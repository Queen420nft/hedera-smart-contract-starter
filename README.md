# [Upgradeable Smart Contract Starter with Proxies and Beacon Proxies](https://github.com/Queen420nft/hedera-smart-contract-starter?tab=readme-ov-file)
_**This document contains the information necessary for use the example Hedera example but without local node.**_

This project provides a hands-on demonstration of using upgradeable smart contracts with OpenZeppelin's upgrades plugins in Hardhat. Specifically, it is a smart contract starter project that illustrates the use of both Proxies and Beacon Proxies.

## Understanding Proxies

A proxy is a smart contract that serves as an intermediary between the user and the logic contract. It forwards calls and data to the logic contract and then returns the results. The address of the logic contract can be updated, thus allowing changes to the contract's behavior over time without changing the contract's address.

## Understanding Beacon Proxies

A Beacon Proxy is a proxy that references a Beacon contract to determine its implementation. By updating the Beacon contract, all associated Beacon Proxies will point to the new implementation, allowing simultaneous upgrades of multiple contracts.

## How It Works

This project contains two contracts: `Topic` and `TopicV2`.

- `Topic` is a simple contract that enables storing and retrieving a Hedera Topic ID.
- `TopicV2` is an enhanced version of `Topic` that introduces an additional feature: the ability to set a message.

We use OpenZeppelin's upgrades plugins to deploy these contracts as upgradeable contracts.

## Setting up Testnet

Before running this project, you need to set up your testnet private key in the `.env` file.

:information_source: [Create a account in portal.hedera.com](https://portal.hedera.com/dashboard)

1. Copy your code from Account `ECDSA`>`HEX Encoded Private Key`


2. Copy it in a `ECDSA_PRIVATE_KEY_TEST` entry to the `.env` file. _Probably the copiler should you mark a error requering a `ECDSA_PRIVATE_KEY_LOCAL`, in this case, you can copy the same private key.

    ```
    ECDSA_PRIVATE_KEY_TEST=your-generated-private-ecdsa-key
    ```

## Running the Project

1. Install the dependencies:

    ```sh
    npm install
    ```

2. Compile the contracts:

    ```sh
    npx hardhat compile
    ```

3. Deploy the `Topic` contract:

    ```sh
    npx hardhat run scripts/create-topic.ts --network testnet
    ```

    This script deploys the `Topic` contract as a proxy contract, initializes it with a Topic ID '0.0.1234', and prints the address of the proxy contract.

4. Upgrade the `Topic` contract to `TopicV2`:

    ```sh
    npx hardhat run scripts/upgrade-topic.ts --network testnet
    ```

    This script upgrades the `Topic` contract to `TopicV2`, sets a new message, and prints the updated message.

5. Deploy the `Topic` contract as a Beacon Proxy:

    ```sh
    npx hardhat run scripts/create-topic-beacon.ts --network testnet
    ```

    This script deploys a Beacon and a Beacon Proxy for the `Topic` contract, initializes the Beacon Proxy with a Topic ID '0.0.1234', and prints the addresses of the Beacon and the Beacon Proxy.

6. Upgrade the `Topic` contract to `TopicV2` for the Beacon Proxy:

    ```sh
    npx hardhat run scripts/upgrade-topic-beacon.ts --network testnet
    ```

    This script upgrades the Beacon to point to `TopicV2`, effectively upgrading all Beacon Proxies. It sets a new message on the Beacon Proxy and prints the updated message.

These scripts illustrate how proxies and beacon proxies allow the contract logic to be upgraded while keeping the contract address constant. The use of beacon proxies also demonstrates the power of simultaneous upgrades across multiple contracts.

## Running the Tests

To test the contracts, we've provided a testing suite in the `test` directory. You can run these tests using the following commands:

1. Compile the contracts (if not already compiled):

    ```sh
    npx hardhat compile
    ```

2. Run the tests:

    ```sh
    npx hardhat test
    ```

The test suite uses Waffle for fast, reliable testing of Ethereum contracts. It tests the base functionality of the `Topic` and `TopicV2` contracts, as well as the upgradeability provided by the proxies.
