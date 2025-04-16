### Experiment 1: Decentralized Certificate Verification
## Aim:
  To develop a smart contract for issuing and verifying academic certificates on Ethereum, preventing forgery and ensuring authenticity.
## Algorithm:
1. Deploy a smart contract where universities can issue certificates.
2. Store a hash of certificate data on-chain.
3. Provide a verification function that checks certificate authenticity.
4. Users can verify the certificate by comparing the stored hash.
## Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract CertificateVerification {
 address public university; // university address
 // Mapping to store issued certficate hashes
mapping(bytes32 => bool) public certificates; 

//event emitted when a certificate is issued
event CertificateIssued(bytes32 indexed certHash);

//construtor sets the deployer (university)
constructor() {
university = msg.sender; 
}

       //function to issue certificate (only university can call)
       function issueCertificate(string memory studentName, string memory degree, uint256 year) public {
           require(msg.sender == university, "Only university can issue certificates");

       // create a unique hash of te certificate details
       bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));

       certificates[certHash] = true;
       emit CertificateIssued(certHash);
   }

   // Function to verify if a certificate exists
   function verifyCertificate(string memory studentName, string memory degree, uint256 year) public view returns (bool) {

       // create a unique hash of te certificate details
       bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));

       certificates[certHash] = true;
       emit CertificateIssued(certHash);
   }

   // Function to verify if a certificate exists
   function verifyCertificate(string memory studentName, string memory degree, uint256 year) public view returns (bool) {
      bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));

      bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));
      return certificates[certHash];
   }
}  
```
# Expected Output:
```
● When the university issues a certificate, it gets stored as a hash.
● A student or employer can verify the certificate by entering the details.
● If valid, it returns true; otherwise, false.
High-Level Overview:
● Used to prevent fake certificates.
● Enables quick verification by employers or other institutions.
● Shows how blockchain can be used in education and credential verification.
```
# Result:
![Screenshot (65)](https://github.com/user-attachments/assets/a08336f6-f274-4d0f-a01c-0dc3c4011d90)
![Screenshot (64)](https://github.com/user-attachments/assets/ac6c6da9-5997-41f2-8e3a-dac28e47f40b)

