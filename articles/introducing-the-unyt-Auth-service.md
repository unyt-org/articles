<!--
	{
		description: "We are introducing the unyt Auth service - your gateway to the Supranet.",
		preview: "TODO",
		date: ~2023-10-20~,
		tag: "Developer",
		author: "auth.unyt.org",
		authorRef: https://auth.unyt.org
	};
-->


# Introducing: The unyt Auth service
We are shipping our decentralized and secure authentication service "unyt Auth," a free-of-charge authentication service that simplifies and enhances the process of endpoint authentication on the Supranet.

As of today - you can join the Supranet, a decentralized network, using your own endpoint identifier and public keys to connect to any of the available relays. Here unyt Auth serves as the entry point to the network, ensuring a secure and reliable connection for users.

## Anonymous Endpoints
unyt Auth provides the flexibility of using anonymous endpoint IDs, making it possible to join the network without leaving too many traces. Users can connect to the Supranet without revealing their real identities, thus ensuring their online privacy.
To create and login as anonymous endpoint the unyt Auth service is not required.

## Certified Endpoints
For those who wish to use verified endpoint IDs, such as @example or @elonmusk, unyt Auth offers an essential service. To achieve this, users must have their authentication signed by unyt Auth service endpoints. Registration and reservation of endpoint IDs can be conveniently performed through unyt.auth's web components and interfaces.


# Authentication
unyt Auth offers users two primary methods for authentication:

**Key Authentication**: This method relies on the handling of the endpoints key pair. It requires loading sensitive information from the private key.

**Password Authentication**: For added convenience, unyt Auth enables users to use a password as a login method. This eliminates the need to exchange the entire private key when switching between devices.

When using the password authentication method, unyt Auth ensures the security of users' private keys. Our service stores the encrypted private keys on custom infrastructure, and provides it to the user after successful verification through password and a second-factor method of choice like OTP, email, or text message. This approach ensures the private key remains confidential and is only accessible to the user.

Unyt Auth places a strong emphasis on user choice and privacy. Users who are concerned about the security of their private data always have the option to handle the private key and exchange process on their own. unyt Auth does not force users to upload any private data, allowing them to assume responsibility for their data security.

# Technical background
unyt Auth can be seamlessly integrated into any application, enhancing security and authentication processes for users. Consider the example of an app called "MyApp" that includes the unyt Auth component on its page. The unyt Auth page features a key store that associates registered endpoints with specific applications.

When a user logs in to "MyApp," the traffic from the anonymous endpoint created when loading the app is routed to the unyt Auth iframe on the same page. This iframe has the capability to encrypt, decrypt, and sign packages. This design ensures that the private key of the user's app-specific endpoint is never exposed to the app itself. Instead, it is securely stored encrypted in the app's cookies, inaccessible to the app. The unyt Auth iframe handles the decryption and authentication process, ensuring the security and privacy of the user's authentication on the Supranet.

# Conclusion
In summary, unyt Auth offers a robust, decentralized authentication solution that empowers users to choose their preferred method of authentication while maintaining a strong focus on privacy and security. Whether you opt for the convenience of password-based authentication or prefer to manage your private key independently, unyt Auth provides a versatile and user-centric authentication service for the modern online world.


## References
* [unyt.org](https://unyt.org)
* [unyt.auth](https://auth.unyt.org)
