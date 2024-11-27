# Lok-Sabha-Data-Analysis-Using-SQL

CREATE TABLE constituencywise_details (
    S_N INT ,               -- Serial number, assumed to be unique
    Candidate VARCHAR(100),           -- Candidate name
    Party VARCHAR(100),               -- Political party
    Evm_Votes INT,                    -- Number of EVM votes
    Postal_Votes INT,                 -- Number of postal votes
    Total_Votes INT,                  -- Total number of votes (EVM + Postal)
    of_Votes FLOAT,                   -- Percentage or ratio of votes (possibly a float)
    Constituency_ID VARCHAR(50)       -- Constituency ID (up to 50 characters)
);

SELECT * FROM constituencywise_details

CREATE TABLE constituencywise_results (
    S_No INT,                               -- Serial number, not unique
    Parliament_Constituency VARCHAR(100),  -- Primary key: Constituency name
    Constituency_Name VARCHAR(100),          -- Constituency name
    Winning_Candidate VARCHAR(150),          -- Winning candidate's name
    Total_Votes INT,                         -- Total number of votes
    Margin INT,                              -- Margin of victory
    Constituency_ID VARCHAR(100),            -- Constituency ID
    Party_ID INT,                            -- Party ID
	PRIMARY KEY (Parliament_Constituency, Constituency_ID) -- Composite primary key
);

SELECT * FROM constituencywise_results

CREATE TABLE partywise_results (
    Party VARCHAR(150),        -- Party name
    Won INT,                   -- Number of wins for the party
    Party_ID INT PRIMARY KEY  -- Unique party ID, serves as the primary key
);

SELECT * FROM partywise_results

CREATE TABLE statewise_results (
    Constituency VARCHAR(100),                -- Constituency name
    Const_No INT,                              -- Constituency number (integer)
    Parliament_Constituency VARCHAR(100) PRIMARY KEY, -- Primary key: parliament constituency
    Leading_Candidate VARCHAR(150),            -- Leading candidate's name
    Trailing_Candidate VARCHAR(150) NULL,      -- Trailing candidate's name (NULL allowed)
    Margin INT,                                -- Margin of victory (integer)
    Status VARCHAR(100),                       -- Status of the election result
    State_ID VARCHAR(100),                     -- State ID (unique identifier for the state)
    State VARCHAR(100)                         -- State name
);

SELECT * FROM statewise_results

CREATE TABLE states (
    State_ID VARCHAR(100) PRIMARY KEY,  -- Unique identifier for the state (Primary key)
    State VARCHAR(100)                  -- Name of the state
);

SELECT * FROM states


--Q1. find total seats

SELECT
	count(distinct parliament_constituency) as Total_Seats
FROM constituencywise_results

--Q2. What is the total number of seats available for elections in each state


SELECT
	s.state as state_name,
	count(distinct cr.parliament_constituency) as total_seats
FROM constituencywise_results as cr
JOIN statewise_results as sr
ON cr.parliament_constituency = sr.parliament_constituency
JOIN states as s
ON s.state_id = sr.state_id
group by 1

--Q3. Total Seats Won by NDA Allianz

SELECT
	SUM(
		CASE
			WHEN
				party in (
				'Bharatiya Janata Party - BJP',
				'Telugu Desam - TDP',
				'Janata Dal  (United) - JD(U)',
				'Shiv Sena - SHS',
				'AJSU Party - AJSUP',
				'Apna Dal (Soneylal) - ADAL',
				'Asom Gana Parishad - AGP',
				'Hindustani Awam Morcha (Secular) - HAMS',
				'Janasena Party - JnP',
				'Janata Dal  (Secular) - JD(S)',
				'Lok Janshakti Party(Ram Vilas) - LJPRV',
				'Nationalist Congress Party - NCP',
				'Rashtriya Lok Dal - RLD',
				'Sikkim Krantikari Morcha - SKM'
				) THEN won
			ELSE 0
		END
	) as NDA_Total_Seats_Won
FROM
	partywise_results

--Q4. Seats Won by NDA Allianz Parties

SELECT 
    party as Party_Name,
    won as Seats_Won
FROM 
    partywise_results
WHERE 
    party in (
		'Bharatiya Janata Party - BJP',
		'Telugu Desam - TDP',
		'Janata Dal  (United) - JD(U)',
		'Shiv Sena - SHS',
		'AJSU Party - AJSUP',
		'Apna Dal (Soneylal) - ADAL',
		'Asom Gana Parishad - AGP',
		'Hindustani Awam Morcha (Secular) - HAMS',
		'Janasena Party - JnP',
		'Janata Dal  (Secular) - JD(S)',
		'Lok Janshakti Party(Ram Vilas) - LJPRV',
		'Nationalist Congress Party - NCP',
		'Rashtriya Lok Dal - RLD',
		'Sikkim Krantikari Morcha - SKM'
		)
ORDER BY Seats_Won DESC


-- Q5. Total Seats Won by I.N.D.I.A. Allianz


SELECT
	SUM(
		CASE
			WHEN party in (
				'Indian National Congress - INC',
                'Aam Aadmi Party - AAAP',
                'All India Trinamool Congress - AITC',
                'Bharat Adivasi Party - BHRTADVSIP',
                'Communist Party of India  (Marxist) - CPI(M)',
                'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
                'Communist Party of India - CPI',
                'Dravida Munnetra Kazhagam - DMK',
                'Indian Union Muslim League - IUML',
                'Nat`Jammu & Kashmir National Conference - JKN',
                'Jharkhand Mukti Morcha - JMM',
                'Jammu & Kashmir National Conference - JKN',
                'Kerala Congress - KEC',
                'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
                'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
                'Rashtriya Janata Dal - RJD',
                'Rashtriya Loktantrik Party - RLTP',
                'Revolutionary Socialist Party - RSP',
                'Samajwadi Party - SP',
                'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
                'Viduthalai Chiruthaigal Katchi - VCK'
			) THEN won
			ELSE 0
		END
	) as INDIA_Total_Seats_Won
FROM
	partywise_results

-- Q6. Seats Won by I.N.D.I.A. Allianz Parties


SELECT 
    party as Party_Name,
    won as Seats_Won
FROM 
    partywise_results
WHERE 
    party IN (
		'Indian National Congress - INC',
        'Aam Aadmi Party - AAAP',
        'All India Trinamool Congress - AITC',
        'Bharat Adivasi Party - BHRTADVSIP',
        'Communist Party of India  (Marxist) - CPI(M)',
        'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
        'Communist Party of India - CPI',
        'Dravida Munnetra Kazhagam - DMK',
		'Indian Union Muslim League - IUML',
        'Nat`Jammu & Kashmir National Conference - JKN',
        'Jharkhand Mukti Morcha - JMM',
        'Jammu & Kashmir National Conference - JKN',
        'Kerala Congress - KEC',
        'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
        'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
        'Rashtriya Janata Dal - RJD',
        'Rashtriya Loktantrik Party - RLTP',
		'Revolutionary Socialist Party - RSP',
        'Samajwadi Party - SP',
        'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
        'Viduthalai Chiruthaigal Katchi - VCK'
    )
ORDER BY Seats_Won DESC

-- Q7. Add new column field in table partywise_results to get the Party Allianz as NDA, I.N.D.I.A and OTHER


ALTER TABLE partywise_results
ADD party_alliance VARCHAR(50);


--I.N.D.I.A ALLAINCE

UPDATE partywise_results
SET party_alliance = 'I.N.D.I.A'
WHERE party IN (
    'Indian National Congress - INC',
    'Aam Aadmi Party - AAAP',
    'All India Trinamool Congress - AITC',
    'Bharat Adivasi Party - BHRTADVSIP',
    'Communist Party of India  (Marxist) - CPI(M)',
    'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
    'Communist Party of India - CPI',
    'Dravida Munnetra Kazhagam - DMK',	
    'Indian Union Muslim League - IUML',
    'Jammu & Kashmir National Conference - JKN',
    'Jharkhand Mukti Morcha - JMM',
    'Kerala Congress - KEC',
    'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
    'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
    'Rashtriya Janata Dal - RJD',
    'Rashtriya Loktantrik Party - RLTP',
    'Revolutionary Socialist Party - RSP',
    'Samajwadi Party - SP',
    'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
    'Viduthalai Chiruthaigal Katchi - VCK'
);

--NDA ALLAINCE

UPDATE partywise_results
SET party_alliance = 'NDA'
WHERE party IN (
    'Bharatiya Janata Party - BJP',
    'Telugu Desam - TDP',
    'Janata Dal  (United) - JD(U)',
    'Shiv Sena - SHS',
    'AJSU Party - AJSUP',
    'Apna Dal (Soneylal) - ADAL',
    'Asom Gana Parishad - AGP',
    'Hindustani Awam Morcha (Secular) - HAMS',
    'Janasena Party - JnP',
    'Janata Dal  (Secular) - JD(S)',
    'Lok Janshakti Party(Ram Vilas) - LJPRV',
    'Nationalist Congress Party - NCP',
    'Rashtriya Lok Dal - RLD',
    'Sikkim Krantikari Morcha - SKM'
);

--OTHER

UPDATE partywise_results
SET party_alliance = 'OTHER'
WHERE party_alliance IS NULL;


SELECT * FROM partywise_results

--Q8. Which party alliance (NDA, I.N.D.I.A, or OTHER) won the most seats across all states?

SELECT
	p.party_alliance,
	count(cr.constituency_ID) as Seats_won
FROM constituencywise_results cr
JOIN partywise_results p
on p.party_id = cr.party_id
GROUP BY 
	p.party_alliance
ORDER by
	Seats_won desc

-- Q9. Winning candidate's name, their party name, total votes, and the margin of victory for a specific state and constituency?

SELECT
	cr.winning_candidate,
	p.party,
	p.party_alliance,
	cr.total_votes,
	cr.margin,
	cr.constituency_id,
	s.state
from constituencywise_results as cr
JOIN partywise_results as p 
on cr.party_id = p.party_id
JOIN statewise_results as sr
on sr.parliament_constituency = cr.parliament_constituency
JOIN states s
on s.state_id = sr.state_id
where
	s.state = 'Uttar Pradesh'
	and
	cr.constituency_name = 'AMETHI'

--Q10. What is the distribution of EVM votes versus postal votes for candidates in a specific constituency?

SELECT
	cd.candidate,
	cd.party,
	cd.evm_votes,
	cd.postal_votes,
	cd.total_votes,
	cr.constituency_name
from constituencywise_details cd
join constituencywise_results cr
on cd.constituency_id = cr.constituency_id
where
	cr.constituency_name = 'MATHURA'
order by cd.total_votes DESC

--Q11. Which parties won the most seats in s State, and how many seats did each party win?

SELECT
	p.party,
	count(cr.constituency_id) as seats_won
FROM constituencywise_results as cr
JOIN partywise_results as p
on p.party_id = cr.party_id
JOIN statewise_results as sr
on cr.parliament_constituency = sr.parliament_constituency
JOIN states as s
on s.state_id = sr.state_id
WHERE
	s.state = 'Maharashtra'
group by 
	p.party
order by
	seats_won DESC


--Q12. What is the total number of seats won by each party alliance (NDA, I.N.D.I.A, and OTHER) in each state for the India Elections 2024

SELECT
	s.state as state_name,
	sum(case when p.party_alliance = 'NDA' THEN 1 ELSE 0 END) as NDA_Seats_Won,
	sum(CASE when p.party_alliance = 'I.N.D.I.A' THEN 1 ELSE 0 END) AS INDIA_Seats_Won,
	sum(CASE when p.party_alliance = 'OTHER' THEN 1 ELSE 0 END) AS OTHER_Seats_Won
FROM constituencywise_results as cr
JOIN partywise_results as p
on cr.party_id = p.party_id
JOIN statewise_results as sr
on sr.parliament_constituency = cr.parliament_constituency
JOIN states as s
on s.state_id = sr.state_id
GROUP by s.state
ORDER by s.state

--Q13. Which candidate received the highest number of EVM votes in each constituency (Top 10)?

SELECT
	cr.constituency_name,
	cr.constituency_id,
	cd.candidate,
	cd.evm_votes
from constituencywise_details as cd
JOIN constituencywise_results as cr
on cd.constituency_id = cr.constituency_id
WHERE
	cd.evm_votes = (select max(cd1.evm_votes)
					from constituencywise_details cd1
					where cd1.constituency_id = cd.constituency_id)
order by evm_votes desc
limit 10

--Q14. Which candidate won and which candidate was the runner-up in each constituency of State for the 2024 elections?

with rankedcandidates as
(
SELECT
	cd.constituency_id,
	cd.candidate,
	cd.party,
	cd.evm_votes,
	cd.postal_votes,
	cd.evm_votes + cd.postal_votes as total_votes,
	row_number() over(partition by cd.constituency_id order by cd.evm_votes+cd.postal_votes desc) as voterank
FROM constituencywise_details as cd
join constituencywise_results as cr
on cr.constituency_id = cd.constituency_id
JOIN statewise_results as sr
on sr.parliament_constituency = cr.parliament_constituency
JOIN states as s
on s.state_id = sr.state_id
WHERE
	s.state = 'Maharashtra'
)
SELECT
	cr.constituency_name,
	max(case when rc.voterank = 1 then rc.candidate end) as winning_candidate,
	max(case when rc.voterank = 2 then rc.candidate end) as runnerup_candidate
FROM rankedcandidates as rc
join constituencywise_results as cr
on cr.constituency_id = rc.constituency_id
group by
	cr.constituency_name
order by 
	cr.constituency_name

	
--Q15. For the state of Maharashtra, what are the total number of seats, total number of candidates, total number of parties,
--     total votes (including EVM and postal), and the breakdown of EVM and postal votes? 

 SELECT
	count(distinct cr.constituency_id) as total_seats,
	count(distinct cd.candidate) as total_candidates,
	count(distinct p.party) as total_party,
	sum(cd.evm_votes+cd.postal_votes) as total_votes,
	sum(cd.evm_votes) as total_evm_votes,
	sum(cd.postal_votes) as total_postal_votes
FROM constituencywise_results as cr
join constituencywise_details as cd
on cr.constituency_id = cd.constituency_id
join statewise_results as sr
on sr.parliament_constituency = cr.parliament_constituency
join states as s
on s.state_id = sr.state_id
join partywise_results as p
on cr.party_id = p.party_id
WHERE
	s.state = 'Maharashtra'
