# cicket_stats

CREATE DATABASE cricket_stats;
USE cricket_stats;

CREATE TABLE teams (
team_id INT PRIMARY KEY,
team_name VARCHAR(50)
);

CREATE TABLE players (
    player_id INT PRIMARY KEY,
    player_name VARCHAR(50),
    team_id INT,
    role VARCHAR(20),
    FOREIGN KEY (team_id) REFERENCES teams(team_id)
);

CREATE TABLE matches (
match_id INT PRIMARY KEY,
match_date DATE,
team1_id INT,
team2_id INT,
venue VARCHAR (50),
FOREIGN KEY (team1_id) REFERENCES teams(team_id),
    FOREIGN KEY (team2_id) REFERENCES teams(team_id)
);

CREATE TABLE batting_stats (
match_id INT,
player_id INT,
runs INT,
balls INT, 
fours INT,
sixes INT,
PRIMARY KEY (match_id, player_id),
    FOREIGN KEY (match_id) REFERENCES matches(match_id),
    FOREIGN KEY (player_id) REFERENCES players(player_id)
);

CREATE TABLE bowling_stats (
    match_id INT,
    player_id INT,
    overs DECIMAL(4,1),
    runs_conceded INT,
    wickets INT,
    PRIMARY KEY (match_id, player_id),
    FOREIGN KEY (match_id) REFERENCES matches(match_id),
    FOREIGN KEY (player_id) REFERENCES players(player_id)
);

INSERT INTO teams
VALUES
(1, 'INDIA'),
(2, 'AUSTRALIA');

INSERT INTO players
VALUES
(10, 'Virat', 1, 'Batsman'),
(11, 'Rohit', 1, 'Batsman'),
(20, 'Warner', 2, 'Batsman'),
(21, 'Cummins', 2, 'Bowler');

INSERT INTO matches
VALUES
(1001, '2024-11-05', 1, 2, 'Mumbai');

INSERT INTO batting_stats
Values
(1001, 10, 82, 49, 6, 4),
(1001, 11, 45, 32, 5, 1),
(1001, 20, 53, 35, 4, 3);

INSERT INTO bowling_stats VALUES
(1001, 21, 4.0, 30, 2);

SELECT * FROM players;

SELECT p.player_name, b.runs, b.balls, b.fours, b.sixes
FROM batting_stats b
JOIN players p ON b.player_id = p.player_id
WHERE b.match_id = 1001;

SELECT t.team_name, SUM(b.runs) AS total_runs
FROM batting_stats b
JOIN players p ON b.player_id = p.player_id
JOIN teams t ON p.team_id = t.team_id
WHERE t.team_id = 1
GROUP BY t.team_name;

SELECT p.player_name, b.runs
FROM batting_stats b
JOIN players p ON p.player_id = b.player_id
WHERE b.match_id = 1001
ORDER BY b.runs DESC
LIMIT 1;

SELECT p.player_name, bw.wickets
FROM bowling_stats bw
JOIN players p ON p.player_id = bw.player_id
WHERE bw.match_id = 1001
ORDER BY bw.wickets DESC
LIMIT 1;
