tags:: distributed-network, content-addressing, file-system, ipfs

- category:: [[computer-science]]
- # IPFS
	- Open protocols for addressing, routing, and transferring data on the web, built on the ideas of content addressing and peer-to-peer networking.
	- ## Defining IPFS
		- An implementation of IPFS protocol [specifications](https://github.com/ipfs/specs)[](https://github.com/ipfs/specs) [(opens new window)](https://github.com/ipfs/specs), such as Kubo. Learn more about [the principles that define an IPFS implementation](https://docs.ipfs.tech/concepts/implementations/).
		- A decentralized network composed of IPFS nodes that is open and participatory.
		- A modular suite of protocols and standards for organizing and transferring content-addressed data.
	- ## What IPFS Isn't
		- A _storage provider_: While there are storage providers built with IPFS support (typically known as _pinning services_), IPFS itself is a protocol, not a provider.
		- _A cloud service provider_: IPFS can be deployed on and complement cloud infrastructure, but it in of itself is not a cloud service provider.
	- ## Problems IPFS Solves
		- ### Verifiability
			- IPFS uses cryptographic hashes to verify the authenticity and integrity of files, making it difficult for malicious actors to tamper with or delete files
		- ### Resiliance
			- IPFS has no single point of failure, and users do not need to trust each other. In other words, the failure of a single or even multiple nodes in the network does not affect the functioning of the entire network, and an IPFS node can fetch data from the network as long as at least one other node in the network has that data, regardless of its location