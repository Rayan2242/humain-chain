# humain-chain
web3 project using blockchain to check ai content
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract HumainChaine is ERC721URIStorage, Ownable {
    uint256 public tokenCounter;

    struct ContentCert {
        string contentHash;
        string platform;
        string contentLink;
        string tag;
    }

    mapping(uint256 => ContentCert) public certificates;

    event ContentCertified(address indexed user, uint256 indexed tokenId, string tag, string platform);

    constructor() ERC721("HumainChaine", "HCH") {
        tokenCounter = 0;
    }

    function mintCertificate(
        string memory _contentHash,
        string memory _platform,
        string memory _contentLink,
        string memory _tag,
        string memory _tokenURI
    ) public {
        uint256 tokenId = tokenCounter;
        _safeMint(msg.sender, tokenId);
        _setTokenURI(tokenId, _tokenURI);

        certificates[tokenId] = ContentCert(_contentHash, _platform, _contentLink, _tag);
        emit ContentCertified(msg.sender, tokenId, _tag, _platform);
        tokenCounter++;
    }

    function getCertificate(uint256 _tokenId) public view returns (ContentCert memory) {
        return certificates[_tokenId];
    }
}

process.exitCode

const hre = require("hardhat");

async function main() {
  const HumainChaine = await hre.ethers.getContractFactory("HumainChaine");
  const contract = await HumainChaine.deploy();
  await contract.deployed();

  console.log("Contract deployed to:", contract.address);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});

testing :
const { expect } = require("chai");

describe("HumainChaine", function () {
  it("should mint a certificate", async function () {
    const [owner] = await ethers.getSigners();
    const Contract = await ethers.getContractFactory("HumainChaine");
    const hc = await Contract.deploy();
    await hc.deployed();

    const hash = ethers.utils.sha256(ethers.utils.toUtf8Bytes("hello world"));
    await hc.mintCertificate(hash, "Twitter", "https://link", "human", "ipfs://test");

    const cert = await hc.getCertificate(0);
    expect(cert.contentHash).to.equal(hash);
  });
});
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract HumainChaine is ERC721URIStorage, Ownable {
    uint256 public tokenCounter;

    struct ContentCert {
        string contentHash;
        string platform;
        string contentLink;
        string tag;
    }
