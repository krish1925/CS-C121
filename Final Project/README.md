# Pseudoalignment Implementation Report

**Author**: Krish Patel  

## Introduction
Pseudoalignment is a computational technique used in the analysis of RNA-seq data to map reads to transcripts without requiring full alignment. The goal of this project was to implement a pseudoalignment algorithm that reads RNA-seq data in FASTA format, constructs equivalence classes of transcripts based on k-mers, and summarizes the results. This README outlines the approach, design choices, results, and analysis of the pseudoalignment implementation.

## Approach and Design Choices

### 1. Data Loading
- **FASTA Parsing**: RNA-seq reads and transcriptome sequences were loaded from FASTA files, handling multi-line sequences and ignoring lines starting with '>'.
- **Data Cleaning**: Sequences containing 'N' were filtered out to avoid issues during k-mer generation.

### 2. K-mer Mapping
- **Sliding Window Approach**: A sliding window of length 30 was used to generate k-mers from the transcriptome sequences.
- **Hash Map**: A hash map (dictionary) was used to store k-mers as keys, with sets of transcript IDs as values, facilitating quick lookups during the alignment process.

### 3. Handling Ambiguous Nucleotides ('N')
- **Substitution Strategy**: Reads containing 'N' were processed by testing all possible nucleotide substitutions (A, T, C, G) for a valid k-mer match.
- **Skipping Reads with Multiple 'N's**: Reads with more than one 'N' were skipped to maintain efficiency and accuracy.

### 4. Equivalence Class Generation
- **Reverse Complement Handling**: Both the read and its reverse complement were considered to account for reverse strand mappings.
- **Intersection of Transcript Sets**: For each k-mer in a read, the intersection of transcript sets was taken to form the equivalence class.

### 5. Counting Equivalence Classes
- **Dictionary of Equivalence Classes**: A dictionary was used to count occurrences of each unique equivalence class.
- **Tracking Unmapped Reads**: Reads that didn’t map to any transcript were tracked separately.

## Results and Analysis

### Key Results:
- **Number of Reads that Didn’t Map to Anything**: 231,385
- **Number of Unique Equivalence Classes**: 10,273
- **Mean Number of Items in Equivalence Classes**: 5.04
- **Median Number of Items in Equivalence Classes**: 3.00
- **Standard Deviation of Items in Equivalence Classes**: 5.24

### 1. Histogram Analysis
The histogram of counts vs. number of items in equivalence classes indicates that most reads map to a few transcripts. The skewed distribution is typical of RNA-seq data, where highly expressed transcripts dominate.

### 2. Box Plot Analysis
The box plot highlights the central tendency and variability in the number of items within equivalence classes, with many outliers suggesting that a subset of equivalence classes contains significantly more items.

### 3. Bar Plot Analysis
The bar plot demonstrates a declining trend in the number of equivalence classes as the number of items increases, a characteristic observation in RNA-seq data.

## Design Choices and Rationale
- **K-mer Length (k=30)**: This length provided a good balance between specificity and computational efficiency.
- **Handling Reverse Complements**: Considering both the read and its reverse complement ensured accuracy for reverse strand mappings.
- **Hash Map for K-mer Storage**: Efficient storage and quick lookups were critical for pseudoalignment performance.
- **Intersection of Transcript Sets**: Intersecting transcript sets for each k-mer ensured accurate mappings.

## Conclusion
This pseudoalignment implementation successfully mapped RNA-seq reads to transcripts and generated equivalence classes. The analysis of the results provided valuable insights into the transcriptome structure. Future improvements may involve optimizing unmapped read handling and exploring different k-mer lengths to enhance accuracy.

