SELECT * FROM REDDIT_COMMENT_ANALYSIS.COMMENT_ANALYSIS.COMMENTS_ANALYSIS

ALTER TABLE reddit_comment_analysis.comment_analysis.comments_analysis
ADD COLUMN "subreddit" STRING;

DELETE FROM COMMENTS_ANALYSIS
WHERE "subreddit" = 'usanews'

// Sentiment Distrubution
SELECT
    "subreddit", 
    "label", 
    COUNT(*) AS sentiment_count
FROM comments_analysis
GROUP BY "subreddit", "label"
ORDER BY "subreddit", "label";

// Overall Sentiment Proportion
SELECT 
    "label", 
    COUNT(*) * 100.0 / SUM(COUNT(*)) OVER () AS percentage
FROM comments_analysis
GROUP BY "label";


// Overall Sentiment Heatmap
SELECT 
    "subreddit", 
    "label", 
    COUNT(*) AS sentiment_count
FROM comments_analysis
GROUP BY "subreddit", "label";

// Top Communities by Sentiment
SELECT 
    "subreddit", 
    "label", 
    COUNT(*) AS sentiment_count
FROM comments_analysis
WHERE "label" = 'POSITIVE'  -- Change this to 'negative' or 'neutral' as needed
GROUP BY "subreddit", "label"
ORDER BY sentiment_count DESC
LIMIT 10;


// Sentiment Word Cloud
WITH words AS (
  SELECT LOWER(value::string) AS word
  FROM comments_analysis,
  LATERAL FLATTEN(input => SPLIT("text", ' '))  -- Split COMMENT_TEXT into individual words
)
SELECT word, COUNT(*) AS word_count
FROM words
JOIN comments_analysis ca ON TRUE  -- No need for COMMENT_ID; just join with the entire dataset
WHERE "label" = 'NEGATIVE'  -- Adjust sentiment filter (e.g., POSITIVE, NEGATIVE)
  AND word NOT IN ('the', 'and', 'is', 'of', 'to', 'a', 'in', 'on', 'for')  -- Optional: exclude common stopwords
GROUP BY word
ORDER BY word_count DESC
LIMIT 100;

