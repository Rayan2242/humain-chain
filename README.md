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
