# Algorand Auction Demo

This demo is an on-chain NFT auction using smart contracts on the Algorand blockchain.

## Usage

The file `auction/operations.py` provides a set of functions that can be used to create and interact
with auctions. See that file for documentation.

## Development Setup

This repo requires Python 3.6 or higher. We recommend you use a Python virtual environment to install
the required dependencies.

Set up venv (one time):
 * `python3 -m venv venv`

Active venv:
 * `. venv/bin/activate` (if your shell is bash/zsh)
 * `. venv/bin/activate.fish` (if your shell is fish)

Install dependencies:
* `pip install -r requirements.txt`

Run tests:
* First, start an instance of [sandbox](https://github.com/algorand/sandbox) (requires Docker): `./sandbox up nightly`
* `pytest`
* When finished, the sandbox can be stopped with `./sandbox down`

Format code:
* `black .`

## Explanation

### Alice and Bob's auction
Meet Alice and Bob. Alice is a talented artist, looking to grow her fanbase and her value as an artist. Bob is a developer and Alice’s good friend. He wants to help her out.

**Meet Alice and Bob**
Alice usually sells her artwork through personal connections and sometimes by advertising on social media. One of her digital art pieces sells for $100, on average, using her current sale techniques. She thinks she could make a lot more if she were able to scale and reach a wider audience.

**Alice and Bob’s auction design**
Alice and Bob think through the features for their dApp. They list off the following requirements:

1. Sellers must be able to create a new auction for each piece of artwork. The artwork must be held by the contract after the auction begins and until the auction closes.
The auction can be closed before it begins, in which case the artwork will be returned to the seller.
2. Sellers can specify a reserve price, which if not met, will return the artwork to them.
3. For each bid, if the new bid is higher than the previous bid, the previous bid will be refunded to the previous bidder and the new bid will be recorded and held by the contract.
4. At the end of a successful auction, where the reserve price was met, the highest bidder will receive the artwork and the seller will receive the full bid amount.
Now onto the next section where you will learn how to build the auction dApp using Python and the PyTeal library.

**Application overview**

The auction demo application uses a smart contract to auction off a non-fungible token. The smart contract covers four basic methods that are used to achieve this functionality.

The **first method** creates the smart contract and initializes the auction state on the blockchain, which includes the auction creator’s account, the specific NFT ID to auction, the NFT seller’s account, the start and end times of the auction, the reserve amount for the auction, and the minimum bid increment.
The **second method** completes the auction set up by funding the smart contract with a small amount of algos (to fulfill minimum balance requirements and to pay transaction fees) and by moving the NFT into the smart contract.
The **third method** constructs the bid scenario. This involves the potential buyer sending the bid in algos to the smart contract. If the bid is successful, the contract will hold the algos. Bidding is only allowed between the beginning and end times of the auction. If the bid supplants a previous bid, the contract automatically refunds the previous higher bidder’s algos.
The **fourth and final method** of the contract allows the auction to be closed out, which will either award the NFT to the highest bidder and transfer the algos to the seller or return the NFT to the seller and close out any remaining algos to the seller.
