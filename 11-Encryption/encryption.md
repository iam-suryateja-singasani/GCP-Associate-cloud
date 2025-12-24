# Different data states
    you may have one or more harddisk which are attached to an instance and instance is writing data to harddisk

## Data at rest:    
        Stored on a device or a backup
        eg: data on a hard disk, in a database, backups and archives
## Data in motion:
        Being transferred across a network, which means writing some data  from my compute instace to the disk here data is transfered across the network also called **Data in transit**
        eg: Data copied from on-premise to cloud storage, 
        an application talking to a database    
    data in motion contains 2 types         
        1)  In and Out of Cloud (from Internet): security is high priority
        2) within cloud:  here security consideration lower level
## Data in Use:
        Active data Processed in a non-persistent state  
        eg: data in your RAM means which is active data       

# Encryption        
    if you store data as it is, what would happen if an unauthorized entity access to it?
        imagine losing an unencrypted hard disk
    **First law of security**: Defense in Depth
    Typically, enterprises encrypt all data    
        1) Data on your hard disk
        2) Data in your databases
        3) Data on your file server
    Is it sufficient if you encript data at rest?
        NO we should Encript data in transit: between application to database as well
**2 types of encryption**
   **Symmetric Key Encryption**: means symmetric encryption algorithms use the same key for encryption and decryption   
        there are key factors 
        1) choose the right encryption algorithm
        2) How do we secure the encryption key?
        3) How do we share the encryption key?      

   **Asymmetric Key Encryption**: means different key for encryption and different key for decryption which menas 2 keys public and private key which is also called public Key Cryptography
   Encrypt data with Public Key and decrypt with Private Key
   so You can share Public Key with everybody and keep the Private key with you.

**Cloud KMS**: Create and manage Cryptograohic keys (symmetric and asymmetric), can control their use in your applications and GCP services. 
KMS provides an API to encrypt, decrypt or sign data
Use existing cryptographic keys created on premises
Integrates with almost all GCP services that need data encryption:
    Google-managed key: No configuration required google manages for you
    Customer-managed Key: Use Key from KMS create key in kms 
    Customer-supplied Key: Provide your own key create key from outside

    when we start creating KMS keys first we should create key ring and then you can attach multiple keys 
    you can create regional or global key