# Film Data Cleaning Project (MySQL)

This project involves duplicating, cleaning, and preparing a dataset named `film_data` for further analysis. The operations were performed using **MySQL**, with a focus on removing duplicates, correcting formatting issues, and ensuring data consistency.

**🔗 [Project 1: Film Data Cleaning Project (MySQL)](https://github.com/Abigail-Jatto/Abigail-portfolio.git)**

---

## Database & Table Setup

1. **Select database**  
   ```sql
   USE world;
   ```

2. **Duplicate the existing dataset**  
   ```sql
   CREATE TABLE films LIKE film_data;
   INSERT INTO films SELECT * FROM film_data;
   ```

3. **Count total rows**  
   ```sql
   SELECT COUNT(*) AS total_rows FROM films;
   ```

---

## Data Cleaning Steps

### 1. **Remove unwanted column**
```sql
ALTER TABLE films DROP COLUMN `title_year_[0]`;
```

### 2. **Assign a unique identifier**
```sql
ALTER TABLE films ADD COLUMN movie_id INT AUTO_INCREMENT PRIMARY KEY;
```

### 3. **Detect and handle duplicate rows**
```sql
-- Check for duplicates
SELECT movie_title, COUNT(movie_title) AS movie_counts
FROM films
GROUP BY movie_title
HAVING COUNT(movie_title) > 1;

-- View a specific duplicate
SELECT * FROM films
WHERE movie_title = 'Avengers: Age of Ultron?�';

-- Delete the duplicate entry
DELETE FROM films WHERE movie_id = 8;
```

### 4. **Clean formatting issues**
```sql
-- Enable updates
SET SQL_SAFE_UPDATES = 0;

-- Remove unwanted characters from movie titles
UPDATE films
SET movie_title = TRIM(BOTH "?�" FROM movie_title);
```

---

## ⏱️ Handling Missing or Invalid Duration

### 1. **Check for invalid durations**
```sql
SELECT AVG(duration) AS avg_duration
FROM films
WHERE duration = ' ';
```

### 2. **Replace blanks with average duration**
```sql
UPDATE films
SET duration = @avg_duration
WHERE duration = ' ';
```

---

## 🧑‍🎤 Rename & Clean Columns

### 1. **Standardize column names**
```sql
ALTER TABLE films  
RENAME COLUMN DIRECTOR_facebook_likes TO director_facebook_likes,
RENAME COLUMN ACTOR_1_facebook_likes TO actor_1_facebook_likes,
RENAME COLUMN Cast_Total_facebook_likes TO cast_total_facebook_likes,
RENAME COLUMN ACTOR_2_facebook_likes TO actor_2_facebook_likes;
```

### 2. **Remove quotes from numeric strings**
```sql
UPDATE films
SET director_facebook_likes = TRIM(BOTH '"' FROM director_facebook_likes)
WHERE director_facebook_likes REGEXP '^".*"$';

ALTER TABLE films MODIFY director_facebook_likes INT;
```

---

## 📊 Final Data Check

```sql
SELECT * FROM films;
```

---

## ✅ Summary

This SQL project showcases basic yet essential steps in preparing a raw dataset for analysis, including:

- Removing duplicate records and columns  
- Assigning unique identifiers  
- Cleaning inconsistent values  
- Renaming and reformatting columns

