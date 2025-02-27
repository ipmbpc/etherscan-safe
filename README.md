# Use SAFE on etherscan

For this example, we will use the SAFE wallet contract created on the POLYGON network at the address:

[0x5Eb7e50857A0b2755a9C79849FADA46C55C1bd51](https://polygonscan.com/address/0x5Eb7e50857A0b2755a9C79849FADA46C55C1bd51)

Multisig: 2/4

## Purpose

This guide demonstrates how to approve an ERC721 contract using a SAFE wallet through three parts.

### PART A: Approve an ERC721 contract

1. Visit the ERC721 contract address (example: [0x6878c8851e78ab10777753f5c79a5478bd1b2a3b](https://polygonscan.com/address/0x6878c8851e78ab10777753f5c79a5478bd1b2a3b))
2. Click on "Contract" tab and then "Write Contract"
3. Connect your wallet on Polygonscan
4. Locate the `setApprovalForAll` function and insert these inputs:
   - operator: `0x2e50790cDdf3dB93A9f338EE6De6857216cd9aCA`
   - approved: `true`
5. Click "Write" and when the MetaMask window pops up, don't execute - just copy the Data: `0xa22cb46500000000000000000000000050c3bfe4ecf9eb15fb306534f8eeeae58cb0f62f0000000000000000000000000000000000000000000000000000000000000001`
6. The data includes:
   - Function selector: `a22cb465`
   - Operator address
   - Bool state: `1` (true)

### PART B: Prepare Transaction Data

1. Visit your SAFE wallet, click "Contract" and then "Read as Proxy"
2. Locate the `encodeTransactionData` function and insert these parameters:
   - to: ERC721 contract address (`0x6878c8851e78ab10777753f5c79a5478bd1b2a3b`)
   - value: `0`
   - data: Data from Part A
   - operation: `0`
   - safeTxGas: `0`
   - baseGas: `0`
   - gasPrice: `0`
   - gasToken: `0x0000000000000000000000000000000000000000`
   - refundReceiver: `0x0000000000000000000000000000000000000000`
   - nonce: `1` (Click "Read nonce" function, find current nonce and use it)
3. Click "Query" to get bytes output: `0x19014b97f1d3afb1793d24952c0559eaa5037cec9275be6e9cd494701d1306a85a93fa9c4e9e6660361c7b019c998e20af6cc32c870c56a929de420e2be9c38905ab`


### PART C: Execute Transaction

1. Run a web server (e.g., WAMP)
2. Open the gnosis HTML file and replace the message value with the output from PART B
3. Access the gnosis file (http://localhost/verifysignature/gnosis.html)
4. Sign the message from one SAFE owner's wallet, click Inspect, and copy the signature: `0xdd9c29a1ec71401dd50a639e53c322b56363f9d76bbd0fad7810c0c25f7178946b90846e40d35a8d8016c5d4db40a1a1d7fd55d254324ee1528130d94fb693981b`
5. Repeat with other owners

#### Modify Signatures

SAFE wallet signatures need v value modification:
- If v = 27 (1b), change to 31 (1f)
- If v = 28 (1c), change to 32 (20)

In this case, the one signature has v = 27, so change it to 31:
- 27 (1b) → 31 (1f)

Modified signatures:
1. `0xdd9c29a1ec71401dd50a639e53c322b56363f9d76bbd0fad7810c0c25f7178946b90846e40d35a8d8016c5d4db40a1a1d7fd55d254324ee1528130d94fb693981f`

Other signatures:
1. `0x000000000000000000000000Dc8284761eDC5D59995652487Ea8a97cac1CE547000000000000000000000000000000000000000000000000000000000000000001`

#### Execute Transaction

Call `execTransaction` function using "Write As Proxy":
- Use PART B details
- Combine signatures: `0xdd9c29a1ec71401dd50a639e53c322b56363f9d76bbd0fad7810c0c25f7178946b90846e40d35a8d8016c5d4db40a1a1d7fd55d254324ee1528130d94fb693981f000000000000000000000000Dc8284761eDC5D59995652487Ea8a97cac1CE547000000000000000000000000000000000000000000000000000000000000000001`

Please note that the wallet that has the second signature will execute the function, in our case this wallet has the address `0xDc8284761eDC5D59995652487Ea8a97cac1CE547`

#### Transaction Result

[Executed Transaction](https://polygonscan.com/tx/0x2d5eac250a08b978a9825a28f93587dcd85dae5b9d401c8a7a1928442a353294)



