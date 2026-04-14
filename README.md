# Hadoop Word Count for Real Estate Data Analysis

This project demonstrates an automated Hadoop MapReduce word count job applied to a dataset of scraped Peruvian real estate listings. It's designed to analyze common terms, identify frequently mentioned districts, and extract insights relevant to a hypothetical real estate company.

## Project Overview

### Data Source
The project utilizes a dataset compiled from scraped real estate listings from Peruvian property portals. This dataset includes:
*   Text descriptions of properties for sale (`.txt` files).
*   Brochure PDFs containing property details (though the current word count script primarily processes `.txt` and `.csv` files).
*   Additional data in `.csv` format.

### Hypothetical Use Case
The primary goal is to analyze this data to understand market trends and consumer preferences. Specifically, the word count analysis aims to answer questions such as:
*   "What are the most frequently mentioned **districts** in property listings?"
*   "What are the most common **property features** or **keywords** used in descriptions?"
*   "Are there any recurring **price-related terms** or **amenities**?"

This information can help a real estate company make informed decisions regarding marketing strategies, property development, and investment.

## Project Setup and Local Execution

This project is designed to run locally using Docker and Docker Compose, automating the Hadoop job execution.

### Prerequisites
*   **Docker**: Ensure Docker Desktop is installed and running.
*   **Docker Compose**: Typically included with Docker Desktop.

### File Structure
The project workspace contains the following key files and directories:
*   `docker-compose.yml`: Defines the Docker services for the Hadoop cluster.
*   `namenode_entrypoint.sh`: A script that automates the entire Hadoop job process when the `namenode` container starts.
*   `src/WordCount.java`: The Java code implementing the Hadoop MapReduce word count logic.
*   `input-data/`: This directory is where you should place your input `.txt` and `.csv` files for analysis.
*   `output-wordcounter/`: This directory will be created on your host machine to store the results of the word count job.
*   `.gitignore`: Configured to ignore the `output-wordcounter/` directory.

### Running the Project
1.  **Place Input Files**: Add your `.txt` and `.csv` files containing property descriptions or other relevant text data into the `input-data/` directory.
2.  **Start the Hadoop Environment and Run Job**: Open a terminal in the project's root directory and run the following command:
    ```bash
    docker compose up -d
    ```
    This command will:
    *   Start the Hadoop cluster services defined in `docker-compose.yml`.
    *   Execute the `namenode_entrypoint.sh` script automatically within the `namenode` container.
    *   The script will copy your input files, compile the Java code, create a JAR, run the Hadoop word count job, and copy the results to `./output-wordcounter/` on your host.

### Accessing Results
The output of the word count job will be saved in the `./output-wordcounter/` directory on your host machine. You can also view the results directly from within the `namenode` container by running:
```bash
docker exec namenode hdfs dfs -cat /output/part-r-00000
```

## Hadoop Word Count Logic

The project uses a standard Hadoop MapReduce implementation (`src/WordCount.java`) for counting word frequencies.
*   **Mapper**: Reads input lines (from `.txt` and `.csv` files), tokenizes them into words, and emits each word with a count of 1.
*   **Reducer**: Aggregates the counts for each unique word and outputs the final word-to-frequency mapping.
*   **Input Handling**: The job is configured to read from the HDFS `/input` directory, which is populated with files from your local `input-data/` directory.

## For Reports and Presentations

When preparing your written report and PowerPoint presentation, consider including the following information:

### Data Source and Use Case
*   **Data Origin**: Scraped Peruvian real estate listings.
*   **Business Problem**: Analyzing market trends, identifying popular locations (districts), common property features, and keywords to inform business strategy.
*   **Hypothetical Analysis**: Discuss how the word count results can reveal insights, e.g., "The frequent appearance of 'Miraflores' and 'San Isidro' indicates high market activity in these districts." or "Terms like 'vista al mar', 'departamento', 'estreno' are common features."

### Technical Details
*   **Environment**: Docker and Docker Compose for local setup.
*   **Hadoop**: MapReduce framework for distributed data processing.
*   **Automation**: The `namenode_entrypoint.sh` script automates the entire workflow from data loading to job execution.
*   **Input Files**: `.txt` and `.csv` files placed in `input-data/`.

### Presentation Suggestions
*   **Demonstrate Setup**: Show the `docker-compose.yml` and `namenode_entrypoint.sh` files, explaining how they automate the process.
*   **Input Data**: Briefly show examples of input files from `input-data/`.
*   **Hadoop Job Execution**: Explain the MapReduce flow (Mapper -> Reducer).
*   **Results**: Present sample output from `./output-wordcounter/` or via `docker exec`. Discuss the insights derived from the word frequencies.
*   **Code Snippets**: Highlight key parts of `WordCount.java` and `namenode_entrypoint.sh`.
*   **Docker Commands**: Show the `docker compose up -d` command and how to check container status or logs.

This setup provides a robust and automated way to perform word count analysis on your real estate data using Hadoop.
