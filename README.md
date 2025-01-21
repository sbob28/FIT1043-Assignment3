# FIT1043 Assignment 3 

## Assignment Overview
This assignment involves analyzing a compressed dataset (`FIT1043_Dataset.gz`) using command-line tools and R programming to extract insights about tweet sentiments. The dataset is processed to identify key statistics, unique users, temporal ranges, and sentiment analysis for mentions of specific countries (USA and Canada). The outputs include text files, CSV files, and visualizations.

## Files Included
- `FIT1043_Dataset.gz`: The compressed dataset used for analysis.
- `sentiment-USA.csv`: Sentiment data for tweets mentioning the USA.
- `sentiment-Canada.csv`: Sentiment data for tweets mentioning Canada.
- `myText.txt`: Lines containing unconventional variations of the word "France."
- `R_Scripts.R`: R code used for generating visualizations.

## Software and Tools Used
- **Command-Line Tools:**
  - `ls`, `gunzip`, `awk`, `grep`, `wc`, `zgrep`
- **Programming Language:** R with the `ggplot2` library
- **Other Tools:** CSV editors (e.g., Microsoft Excel) for manual verification of output data

## Execution Instructions

### Part A1: Basic File Analysis
1. **Determine File Size:**
   ```bash
   ls -lh FIT1043_Dataset.gz
   ```
2. **Identify Delimiter:**
   ```bash
   gunzip -c FIT1043_Dataset.gz | head -5
   ```
3. **Count Number of Lines:**
   ```bash
   gunzip -c FIT1043_Dataset.gz | wc -l
   ```

### Part A2: User Analysis and France Mentions
1. **Count Unique Users:**
   ```bash
   gunzip -c FIT1043_Dataset.gz | awk -F, '{print $5}' | sort | uniq | wc -l
   ```
2. **Determine Date Range:**
   ```bash
   gunzip -c FIT1043_Dataset.gz | awk -F, '{print $3}' | (head -n 1; tail -n 1)
   ```
3. **Mentions of "France":**
   - Standard Case:
     ```bash
     gunzip -c FIT1043_Dataset.gz | awk -F, '{print $6}' | grep -i -c " france "
     ```
   - Variations:
     ```bash
     gunzip -c FIT1043_Dataset.gz | grep -i " france " | grep -v -e " france " -e " France " | wc -l
     ```
   - Save Output:
     ```bash
     gunzip -c FIT1043_Dataset.gz | grep -i " france " | grep -v -e " france " -e " France " > myText.txt
     ```

### Part A3: Sentiment Analysis
1. **Count Sentiments for USA and Canada:**
   - USA:
     ```bash
     zgrep -i " USA " FIT1043_Dataset.gz | awk -F, '{neg+=($1=="0"); neu+=($1=="2"); pos+=($1=="4")} END {print "USA:", "Negative:", neg, "Neutral:", neu, "Positive:", pos}'
     ```
   - Canada:
     ```bash
     zgrep -i " Canada " FIT1043_Dataset.gz | awk -F, '{neg+=($1=="0"); neu+=($1=="2"); pos+=($1=="4")} END {print "Canada:", "Negative:", neg, "Neutral:", neu, "Positive:", pos}'
     ```
2. **Create CSV Files:**
   ```bash
   echo -e "sentiment,count\nNegative,541\nNeutral,0\nPositive,698" > sentiment-canada.csv
   echo -e "sentiment,count\nNegative,263\nNeutral,0\nPositive,175" > sentiment-USA.csv
   ```

### Part A4: Visualization in R
1. **Install Required Library:**
   ```R
   install.packages("ggplot2")
   ```
2. **R Code Execution:**
   Use the provided `R_Scripts.R` file to generate a side-by-side bar chart for sentiment comparison between USA and Canada.
   ```R
   library(ggplot2)
   usa_sentiment <- read.csv("C:/Users/srivi/Downloads/sentiment-USA.csv")
   canada_sentiment <- read.csv("C:/Users/srivi/Downloads/sentiment-canada.csv")
   usa_sentiment$Country <- "USA"
   canada_sentiment$Country <- "Canada"
   combined_sentiment <- rbind(usa_sentiment, canada_sentiment)
   side_by_side_chart <- ggplot(combined_sentiment, aes(x = Sentiment, y = Count, fill = Country)) +
   geom_bar(stat = "identity", position = "dodge") +
   labs(title = "Sentiment Comparison: USA vs Canada", x = "Sentiment", y = "Count") +
   theme_minimal()
   print(side_by_side_chart)
   ```

## Observations and Results
- **File Size:** 74MB
- **Total Lines in Dataset:** 1,471,793
- **Unique Users:** 626,684
- **Date Range:** April 6, 2009, to June 16, 2009
- **Mentions of France:**
  - Standard Case: 1,091 tweets
  - Variations: 28 tweets
- **Sentiment Analysis:**
  - USA: 263 Negative, 0 Neutral, 175 Positive
  - Canada: 379 Negative, 0 Neutral, 255 Positive

## Notes and Assumptions
- The dataset columns were assumed to be comma-separated.
- Spaces around keywords (e.g., "USA", "Canada") were included to avoid partial matches.
- Neutral sentiments were absent in the dataset, as confirmed through analysis.
- R visualization assumes the use of a compatible IDE (e.g., RStudio).

## References
- **Monash University FIT1043 Lecture Notes**
- **Command Line Documentation:** GNU Coreutils, Awk, and Grep Manuals
- **R Documentation:** ggplot2 Library
