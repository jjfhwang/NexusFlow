# NexusFlow: Decentralized Private Crypto-Portfolio Rebalancing API

NexusFlow provides a decentralized API for automatically rebalancing cryptocurrency portfolios while preserving strategy privacy through zero-knowledge proofs. It leverages zk-SNARKs to execute trading strategies within a secure, verifiable environment on smart contracts, ensuring transparency and trust without revealing the underlying strategy itself. This allows users to benefit from automated portfolio management without exposing their proprietary algorithms or trading secrets to the public blockchain.

The core functionality of NexusFlow revolves around creating and deploying zk-SNARK circuits that encapsulate specific rebalancing strategies. These circuits take portfolio state and market data as private inputs and generate a zero-knowledge proof attesting to the validity of the rebalancing actions, which are then executed via smart contract interactions. This approach offers a significant advantage over traditional centralized rebalancing services, which lack transparency and are vulnerable to manipulation. By utilizing a decentralized and verifiable framework, NexusFlow empowers users with control and confidence in their automated portfolio management.

This repository contains the Python API and associated tooling for developing, deploying, and interacting with the NexusFlow platform. It includes libraries for circuit generation, proof generation and verification, and smart contract interaction. The API is designed to be modular and extensible, allowing developers to customize the rebalancing strategies and integrate with various exchanges and data providers. We aim to create a powerful and secure tool for both individual investors and institutional fund managers seeking to optimize their crypto portfolios with enhanced privacy and verifiable execution.

## Key Features

*   **zk-SNARK powered private strategy execution:** Utilizes Groth16 zk-SNARKs for proving correct execution of rebalancing strategies without revealing the strategy's logic or the portfolio's composition. The implemented circuits are optimized for minimizing gas costs on EVM-compatible blockchains.
*   **Smart contract based verification:** Employs Solidity smart contracts to verify zk-SNARK proofs and execute the rebalancing actions based on the proof's validity. Contracts are designed with security best practices, including reentrancy protection and gas optimization.
*   **Modular architecture for custom strategies:** The API supports the creation of custom rebalancing strategies by allowing developers to define their own circuits and integrate them into the NexusFlow workflow. An abstract base class is provided for defining strategy-specific logic.
*   **Exchange and data provider integration:** Provides an interface for integrating with various cryptocurrency exchanges and data providers for real-time market data and order execution. Currently supports Binance and Coinbase Pro via API keys, with a pluggable architecture for adding additional integrations.
*   **Portfolio risk assessment:** Includes functionality for calculating and assessing portfolio risk metrics (e.g., Sharpe ratio, Value at Risk) based on historical market data. These metrics can be used as inputs for the rebalancing strategies.
*   **Automated proof generation and verification:** Automates the process of generating and verifying zk-SNARK proofs, streamlining the deployment and execution of rebalancing strategies.
*   **Simulation and backtesting:** Offers tools for simulating and backtesting rebalancing strategies using historical data, allowing users to evaluate performance and optimize parameters before deploying to a live environment.

## Technology Stack

*   **Python:** The primary programming language for the API and tooling.
*   **Circom:** A domain-specific language for designing zero-knowledge circuits. Used to define the rebalancing strategies as arithmetic circuits.
*   **SnarkJS:** A JavaScript library for compiling Circom circuits, generating proving keys, and generating and verifying zk-SNARK proofs. SnarkJS is called from the Python API using subprocess execution.
*   **Solidity:** The programming language used for writing the smart contracts that verify the proofs and execute the rebalancing actions.
*   **Web3.py:** A Python library for interacting with Ethereum-compatible blockchains. Used to deploy and interact with the smart contracts.
*   **NumPy:** A Python library for numerical computation, used for portfolio risk assessment and data analysis.
*   **Pandas:** A Python library for data manipulation and analysis, used for managing historical market data.

## Installation

1.  **Clone the repository:**
    git clone https://github.com/jjfhwang/NexusFlow.git
    cd NexusFlow

2.  **Install dependencies:**
    It is highly recommended to use a virtual environment.
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt

3.  **Install Circom and SnarkJS:**
    These tools are required for circuit compilation and proof generation.
    You will need Node.js and npm installed.
    npm install -g circom
    npm install -g snarkjs

4.  **Compile the smart contracts:**
    You will need a Solidity compiler (e.g., solc). Ensure it's in your PATH. Then run:
    python compile_contracts.py

## Configuration

1.  **Environment Variables:**
    The following environment variables need to be set:

    *   `EXCHANGE_API_KEY`: API key for your chosen exchange (e.g., Binance, Coinbase Pro).
    *   `EXCHANGE_API_SECRET`: API secret for your chosen exchange.
    *   `INFURA_PROJECT_ID`: Project ID for Infura, used for connecting to the Ethereum network.
    *   `PRIVATE_KEY`: Your private key for deploying and interacting with smart contracts. Ensure this key has sufficient funds for gas.
    *   `CONTRACT_ADDRESS`: (Optional) If contracts are already deployed, set the smart contract address. Otherwise, they will be deployed during runtime.

2.  **Configuration File:**
    A configuration file (`config.json`) can be used to store other settings, such as the gas price and the desired slippage tolerance. The file should contain JSON formatted data. For example:

    {
        "gas_price": "10 gwei",
        "slippage_tolerance": 0.01
    }

## Usage

1.  **Defining a Rebalancing Strategy:**
    Create a Python class that inherits from the `AbstractRebalancingStrategy` class. Implement the `generate_circuit_input` method to define the inputs for your Circom circuit. The `generate_circuit_input` method should return a dictionary containing the input values.

2.  **Compiling the Circuit:**
    Use the `compile_circuit` function to compile your Circom circuit. This function will generate the necessary R1CS file and WASM file.

3.  **Generating the Proof:**
    Use the `generate_proof` function to generate a zk-SNARK proof for your circuit. This function takes the circuit inputs, the proving key, and the WASM file as input.

4.  **Verifying the Proof:**
    Use the `verify_proof` function to verify the zk-SNARK proof. This function takes the verification key and the proof as input.

5.  **Executing the Rebalancing Action:**
    Call the `execute_rebalancing` function on the deployed smart contract, passing the proof as input. The smart contract will verify the proof and execute the rebalancing action based on the proof's validity.

Example code snippet:
    strategy = MyRebalancingStrategy(exchange="Binance", api_key=os.environ['EXCHANGE_API_KEY'], api_secret=os.environ['EXCHANGE_API_SECRET'])
    inputs = strategy.generate_circuit_input(portfolio_state, market_data)
    proof = generate_proof(inputs, proving_key, wasm_file)
    verify_result = verify_proof(verification_key, proof)
    if verify_result:
        execute_rebalancing(proof)

## Contributing

We welcome contributions to NexusFlow! Please follow these guidelines:

1.  Fork the repository.
2.  Create a new branch for your feature or bug fix.
3.  Write clear and concise code with thorough comments.
4.  Write unit tests for your changes.
5.  Submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](https://github.com/jjfhwang/NexusFlow/blob/main/LICENSE) file for details.

## Acknowledgements

We would like to acknowledge the developers of Circom, SnarkJS, and Web3.py, whose work has been invaluable in building NexusFlow.