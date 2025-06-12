# EX3 Implementation of GSP Algorithm In Python

## AIM

To implement the Generalized Sequential Pattern (GSP) Algorithm in Python and generate frequent sequential patterns.

## PROCEDURE

1. Import necessary libraries: `collections`, `itertools`, `tabulate`, and `matplotlib`.
2. Define `generate_candidates()` function to create candidate sequences.
3. Define `gsp()` function to extract frequent patterns using the GSP algorithm.
4. Create sample datasets for Top Wear, Bottom Wear, and Party Wear.
5. Set minimum support threshold and execute GSP algorithm for all datasets.
6. Display frequent sequential patterns in tabular format.
7. Define `visualize_patterns_line()` function to plot the frequent patterns.
8. Use matplotlib to visualize the results for each wear category.
9. Execute the visualization for Top Wear, Bottom Wear, and Party Wear.
10. Analyze the output and verify the discovered patterns.

## Program:

```python
from collections import defaultdict
from itertools import combinations
from tabulate import tabulate

# Function to generate candidate k-item sequences
def generate_candidates(dataset, k):
    candidates = defaultdict(int)
    for sequence in dataset:
        for itemset in combinations(sequence, k):
            candidates[itemset] += 1
    return candidates

# Function to perform GSP algorithm
def gsp(dataset, min_support):
    # Initialize frequent patterns dictionary
    frequent_patterns = defaultdict(int)
    k = 1
    while True:
        candidates = generate_candidates(dataset, k)
        # Prune candidates with support less than min_support
        candidates = {pattern: support for pattern, support in candidates.items() if support >= min_support}
        if not candidates:
            break
        frequent_patterns.update(candidates)
        k += 1
    return frequent_patterns


# Example dataset for each category
top_wear_data = [
    ["blouse", "t-shirt", "tank_top"],
    ["hoodie", "sweater", "top"],
    ["hoodie"],
    ["hoodie", "sweater"]
]

bottom_wear_data = [
    ["jeans", "trousers", "shorts"],
    ["leggings", "skirt", "chinos"],
]

party_wear_data = [
    ["cocktail_dress", "evening_gown", "blazer"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress"],
    ["party_dress"],
]

# Minimum support threshold
min_support = 2

# Perform GSP algorithm for each category
top_wear_result = gsp(top_wear_data, min_support)
bottom_wear_result = gsp(bottom_wear_data, min_support)
party_wear_result = gsp(party_wear_data, min_support)

# Prepare the output data for tabular format
output_data = []
max_len = max(len(top_wear_result), len(bottom_wear_result), len(party_wear_result))

# Add rows with sequential patterns from each category
for i in range(max_len):
    top_wear_pattern = list(top_wear_result.items())[i] if i < len(top_wear_result) else ("", "")
    bottom_wear_pattern = list(bottom_wear_result.items())[i] if i < len(bottom_wear_result) else ("", "")
    party_wear_pattern = list(party_wear_result.items())[i] if i < len(party_wear_result) else ("", "")
    
    output_data.append([top_wear_pattern[0], top_wear_pattern[1], bottom_wear_pattern[0], bottom_wear_pattern[1], party_wear_pattern[0], party_wear_pattern[1]])

# Define table headers
headers = ["Top Wear Pattern", "Top Wear Support", "Bottom Wear Pattern", "Bottom Wear Support", "Party Wear Pattern", "Party Wear Support"]

# Print the output in table format
print(tabulate(output_data, headers=headers, tablefmt="grid"))
```

## Output:

![image](https://github.com/user-attachments/assets/12f0521a-0073-423e-aae1-49e2bd789492)

## Visualization:
```python
import matplotlib.pyplot as plt

# Function to visualize frequent sequential patterns with a line plot
def visualize_patterns_line(result, category):
    if result:
        patterns = list(result.keys())
        support = list(result.values())

        plt.figure(figsize=(10, 6))
        plt.plot([str(pattern) for pattern in patterns], support, marker='o', linestyle='-', color='blue')
        plt.xlabel('Patterns')
        plt.ylabel('Support Count')
        plt.title(f'Frequent Sequential Patterns - {category}')
        plt.xticks(rotation=90)
        plt.tight_layout()
        plt.show()
    else:
        print(f"No frequent sequential patterns found in {category}.")

# Visualize frequent sequential patterns for each category using a line plot
visualize_patterns_line(top_wear_result, 'Top Wear')
visualize_patterns_line(bottom_wear_result, 'Bottom Wear')
visualize_patterns_line(party_wear_result, 'Party Wear')
```

## Output:

![image](https://github.com/user-attachments/assets/33b26c5e-c1c0-4001-a5d9-24b8853653db)

No frequent sequential patterns found in Bottom Wear.

![image](https://github.com/user-attachments/assets/86ca8cdf-83d9-4c7e-9684-62b4f42fd045)

## Result:

Thus the implementation of the GSP algorithm in python has been successfully executed.
