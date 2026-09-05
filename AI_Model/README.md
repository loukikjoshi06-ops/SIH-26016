# Secure Land Acquisition Document Extraction

## Objective
To train or fine-tune an AI model capable of intelligently reading complex Marathi land documents (like 7-12 extracts), understanding the layout, and extracting the relevant entities into a structured format.

## Security & Privacy Constraints
- **Zero Data Leakage**: Sensitive land records must **never** be sent to third-party APIs (e.g., OpenAI, Google Cloud). All AI inference and training must happen locally or on secure, controlled infrastructure.
- **Blockchain Integration**: Extracted data and document hashes will be stored on a blockchain to ensure immutability, data integrity, and decentralized trust without relying on a central vulnerability point.
