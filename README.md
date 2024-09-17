# Network Community Structure Similarity Index (NCSSI)
A Python implementation of the Network Community Structure Similarity Index (NCSSI) for comparing community structures in weighted networks. This implementation is designed to work with NetworkX graphs and is suitable for inclusion in the NetworkX package.
## Table of Contents
- Introduction
- Installation
- Usage
- Function Signature
- Parameters
- Return Value
- Example
- Detailed Description
- NCSSI Algorithm
- Error Handling
- Contributing
- License
- References
## Introduction
The Network Community Structure Similarity Index (NCSSI) is a metric for quantifying the similarity between two community structures in weighted networks. It considers both community labels and edge weights, making it suitable for comparing complex networks where edge weights carry significant information.
This implementation follows the methodology outlined in the paper:
> Malekzadeh, M., & Long, J. A. (2023). A network community structure similarity index for weighted networks. PLOS ONE, 18(11), e0292018. https://doi.org/10.1371/journal.pone.0292018
## Installation
To use the ncssi function, you need to have Python 3.x and the following packages installed:
- NetworkX (version 2.0 or higher)
- NumPy
You can install these packages using pip:
```bash
pip install networkx numpy
```
## Usage
### Function Signature
```python
def ncssi(
G1,
G2,
bidirectional1=False,
bidirectional2=False,
community_attr='community',
weight_attr='weight'
):
```
### Parameters
G1: networkx.Graph
The first graph to compare.
G2: networkx.Graph
The second graph to compare.
bidirectional1: bool, optional (default=False)
If True, treat G1 as bidirectional (undirected). Edge weights will be considered bidirectionally.
bidirectional2: bool, optional (default=False)
If True, treat G2 as bidirectional (undirected). Edge weights will be considered bidirectionally.
community_attr: str, optional (default='community')
The node attribute name that stores the community information.
weight_attr: str, optional (default='weight')
The edge attribute name that stores the weight information.
### Return Value
ncssi_value: float
The NCSSI value between G1 and G2. It ranges from 0 to 1, where 1 indicates identical community structures, and 0 indicates completely dissimilar structures.
### Example
```python
import networkx as nx
# Create example graphs G1 and G2
G1 = nx.Graph()
G2 = nx.Graph()
# Add nodes and edges to G1
G1.add_nodes_from([1, 2, 3, 4])
G1.add_edges_from([
(1, 2, {'weight': 1}),
(2, 3, {'weight': 2}),
(3, 4, {'weight': 1}),
(4, 1, {'weight': 2})
])
# Assign communities to nodes in G1
nx.set_node_attributes(G1, {1: ['A'], 2: ['A'], 3: ['B'], 4: ['B']}, 'community')
# Add nodes and edges to G2
G2.add_nodes_from([1, 2, 3, 4])
G2.add_edges_from([
(1, 2, {'weight': 1}),
(2, 3, {'weight': 1}),
(3, 4, {'weight': 2}),
(4, 1, {'weight': 2})
])
# Assign communities to nodes in G2
nx.set_node_attributes(G2, {1: ['A'], 2: ['B'], 3: ['B'], 4: ['A']}, 'community')
# Compute NCSSI
ncssi_value = ncssi(G1, G2, community_attr='community', weight_attr='weight')
print(f"NCSSI between G1 and G2: {ncssi_value}")
```
## Detailed Description
### NCSSI Algorithm
The NCSSI algorithm measures the similarity between two community structures by considering both the community labels of nodes and the weights of edges. It follows these main steps:
#### Graph Preparation:
- Extract node-to-community mappings, edge weights, and community definitions from each graph.
- Map node labels to internal indices for consistent processing.
#### Community Matching:
- Compute the Edge-Based Overlapping Score between each pair of communities from the two graphs.
- Find the best matching pairs of communities based on the highest overlap scores.
#### Edit Cost Calculation:
- For each node, compute the edit costs and adjustment factors based on the differences in community membership and edge weights.
- The adjusted edit cost reflects the cost to transform one community structure into another.
#### NCSSI Computation:
- Aggregate the adjusted edit costs and adjustment factors for both graphs.
- Compute the final NCSSI value as 1 - (average adjusted edit cost ratio).
### Error Handling
The ncssi function includes comprehensive error handling to assist users:
#### Type Checking:
- Validates that G1 and G2 are NetworkX graphs.
- Checks that bidirectional1, bidirectional2, community_attr, and weight_attr are of the correct types.
#### Attribute Validation:
- Ensures that all nodes have the specified community attribute.
- Checks that the community attribute is a list or set.
- Verifies that edge weights are numeric.
#### Exceptions:
- Raises TypeError for incorrect parameter types.
- Raises ValueError for missing attributes or incorrect formats.
- Provides informative error messages to guide the user.
### Example of Handling Missing Community Attribute
```python
try:
ncssi_value = ncssi(G1, G2, community_attr='group')
except ValueError as e:
print(f"An error occurred: {e}")
```
Output:
```
An error occurred: Node '1' does not have the community attribute 'group'.
```
## Contributing
Contributions are welcome! If you'd like to contribute to this project, please follow these guidelines:
- Fork the Repository: Click the "Fork" button at the top right of this page to create a copy of this repository in your account.
- Clone the Forked Repository:
```bash
git clone https://github.com/your-username/ncssi.git
cd ncssi
```
- Create a New Branch:
```bash
git checkout -b feature/your-feature-name
```
- Make Changes: Implement your feature or bug fix.
- Write Tests: Ensure your changes are covered by tests.
- Commit and Push:
```bash
git add .
git commit -m "Description of your changes"
git push origin feature/your-feature-name
```
- Create a Pull Request: Go to the original repository and create a pull request from your forked repository.
Please ensure that your code follows the PEP 8 style guidelines and includes appropriate documentation and error handling.
## License
This project is licensed under the MIT License - see the LICENSE file for details.
## References
- Malekzadeh, M., & Long, J. A. (2023). A network community structure similarity index for weighted networks. PLOS ONE, 18(11), e0292018. https://doi.org/10.1371/journal.pone.0292018
- NetworkX Documentation
- NumPy Documentation
