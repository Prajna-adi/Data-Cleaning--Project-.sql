## SQL Project - Data Cleaning
Data Cleaning project on Global Layoffs 

### Project Overview

This project aims to provide detailed insight into the layoffs happend in worldwide from 2020 to 2023

### Data Source

Layoffs Data: The primary dataset used for this analysis is the " Laoffs.csv" file, containing information about contry & company wise layoffs.

``` sql
SELECT*
FROM layoffs;

-- 1. Remove Duplicates
-- 2. Standardise Data
-- 3. Null Values or Blank Values
-- 4. Remove Any Columns

CREATE TABLE layoffs_staging
LIKE layoffs;

SELECT* 
FROM layoffs_staging;

INSERT layoffs_staging
SELECT*
FROM layoffs;


SELECT*
FROM layoffs;

SELECT *,
ROW_NUMBER() OVER(
PARTITION BY company,location,industry, total_laid_off,percentage_laid_off, `date`,
 stage,country,funds_raised_millions) AS row_num
FROM layoffs_staging;

WITH duplicate_cte AS
(
SELECT *,
ROW_NUMBER() OVER(
PARTITION BY company,location,industry, total_laid_off,percentage_laid_off, `date`, 
stage,country,funds_raised_millions) AS row_num
FROM layoffs_staging
)

SELECT*
FROM duplicate_cte
WHERE row_num > 1;

SELECT*
FROM layoffs_staging
WHERE company = 'Casper';

WITH duplicate_cte AS
(
SELECT *,
ROW_NUMBER() OVER(
PARTITION BY company,location,industry, total_laid_off,percentage_laid_off, `date`,
 stage,country,funds_raised_millions) AS row_num
FROM layoffs_staging
)
DELETE
FROM duplicate_cte
WHERE row_num > 1;







CREATE TABLE `layoffs_staging2` (
  `company` text,
  `location` text,
  `industry` text,
  `total_laid_off` int DEFAULT NULL,
  `percentage_laid_off` text,
  `date` text,
  `stage` text,
  `country` text,
  `funds_raised_millions` int DEFAULT NULL,
  `row_num` INT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;


SELECT* 
FROM layoffs_staging2
WHERE row_num > 1;

INSERT INTO layoffs_staging2
SELECT *,
ROW_NUMBER() OVER(
PARTITION BY company,location,industry, total_laid_off,percentage_laid_off, `date`,
 stage,country,funds_raised_millions) AS row_num
FROM layoffs_staging;

DELETE
FROM layoffs_staging2
WHERE row_num > 1;

SELECT*
FROM layoffs_staging2;

-- Standardizing DATA

SELECT company, trim(company)
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET company = trim(company);

SELECT distinct industry
FROM layoffs_staging2
ORDER BY 1;


UPDATE layoffs_staging2
SET industry = 'Crypto'
WHERE industry LIKE 'Crypto%' ;


SELECT distinct country
FROM layoffs_staging2
WHERE Country LIKE 'United States%'
Order BY 1;

SELECT distinct Country, trim( Trailing '.' FROM Country)
FROM layoffs_staging2
Order BY 1;

SELECT `date`,
STR_TO_DATE(`date`, '%m/%d/%Y')
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET `date` = STR_TO_DATE(`date`, '%m/%d/%Y');

SELECT `date`
FROM layoffs_staging2;

ALTER TABLE layoffs_staging2
MODIFY column `date` DATE;

SELECT*
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;

UPDATE layoffs_staging2
SET industry = NULL
WHERE industry = '';


SELECT*
FROM layoffs_staging2
WHERE industry IS NULL
or industry = '';



SELECT*
FROM layoffs_staging2
WHERE company LIKE 'Bally%';



SELECT t1.industry, t2.industry
FROM layoffs_staging2 t1
JOIN layoffs_staging2 t2
	 ON t1.company = t2.company
WHERE (t1.industry IS NULL or t1.industry = '')
AND t2.industry IS NOT NULL;

UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2
     ON t1.company = t2.company
SET t1.industry = t2.industry
WHERE t1.industry IS NULL
AND t2.industry IS NOT NULL;

SELECT*
FROM layoffs_staging2;

SELECT*
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;

DELETE
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;

SELECT*
FROM layoffs_staging2;

ALTER TABLE layoffs_staging2
DROP Column row_num;




