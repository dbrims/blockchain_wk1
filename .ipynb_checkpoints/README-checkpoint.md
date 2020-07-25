# Blockchain Wk 18 homework

### Setting up a network:

1. Create directory for all the files
2. Create a new account for every node in the network.
    * use ./geth account new --datadir node_name
    * the flag --datadir specifies the name of the new account
    * Don't forget what password you used
    * Record the addressess of each account created
3. Create the Genesis Block
    * use putteth to help generate the block
    * name the network
    * configure the block
        * choose clique
        * paste the address of each account you want to be a sealer in the PoA network
        * paste the address of each account you want to be prefunded 
        * Choose a chain ID, 333 is good
        ![genesis_configuration](./screenshots/config_file.png)
        
    * export the configured block, wither in the same directory or into a config directory
4. initialize each node
    * Use ./geth init ./nettymcnetfacenet_config/nettymcnetfacenet.json --datadir node1
    * init tells get to do an initialization
    * the location of the genesis block is specified
    * the flag --datadir says which account (node) to initialize
5. Start the nodes in the network
    * for the first node use the command ./geth --datadir node1/ --rpc --networkid 333 -unlock 'account address' --password password1.sec --mine --allow-insecure-unlock --port 30305  
        * everything must be in one  (very) long line
        * the flag datadir is the location of the data for this node
        * the flag rpc (or soon http) makes this node open visible to the outside network, this only needs to be a single node
        * the flag networkid is the chain ID used in setting up the genesis block (333 in this case)
        * the flag unlock unlocks the address and uses the password from creating of the address which is stored in the .sec file
        * the flag mine starts the node mining
        * geth does not like a http enabled note to be unlocked, so the flag allow-insecure-unlock tells geth to ignore that constraint
        * the flag port gives a port to the enode address, and needs to be unique for each node
        * write downt he enode address for this node
    * for subsequent nodes use the command ./geth --networkid 333 --datadir node2 --mine --unlock 'account address' --password ./password2.sec --allow-insecure-unlock --port 30304 --bootnodes "insert node1 enode address" --ipcdisable
        * most flags are the same 
        * the flag ipcdisable tell the system not to care that two very similar processes are occuring
        * the flag bootnodes tells the subsequent nodes where to find the first node on the network
6. Open Mycrypto and connect to our local network
    * open my crypto and on the lower left of the screen click on "change network"
    ![genesis_configuration](./screenshots/change_network.png)
        * on the lower left again, click on "add custom node"
        ![genesis_configuration](./screenshots/add_node.png)
        * in the dialog box where it says "network" choose custom
        ![genesis_configuration](./screenshots/custom.png)        
        * complete the rest of the field, with network name, chainID (the 333 again), network address (http://127.0.0.1:8545)
        ![genesis_configuration](./screenshots/node_dialog.png)
        * click save
    * click keystore file
        * choose the keystore file for the rpc enabled node and type in the password of the node
        ![genesis_configuration](./screenshots/keystore.png)
        * you should now see the account for that node 
    * do do a transaction, enter the target accountnumber, the amount of eth, etc 
    ![genesis_configuration](./screenshots/transaction.png)
    * transaction complete
    ![genesis_configuration](./screenshots/ETH_transfer.png)
    
7. a common bootnode for all the nodes to use on the machine.
    * use ./bootnode -genkey boot.key
    * -genkey tells it to generate the bootnode key and save it in the file boot.key
    * use ./bootnode -nodekey boot.key -verbosity 9 -addr :30303 to start the bootnode
        * the flad nodekey is the previously generated bootnode key
        * the flag verbosity shows any traffic the bootnode is seeing. 
        * the flag -addr specifies the port for the enode address used by the bootnode
        * copy down the enode address and use it in place of the enode address for the initial node (including in when starting up the node1)
    
8. to allow new nodes to be sealers on the network you use the GETH console, you need at least 50% of nodes to agree to let the new node in
    * ./geth attach http://127.0.0.1:8545 attaches the console to the rpc enabled node
        * clique.propose("new node address", true), to tell the node to agree to add this new sealer
        * clique.getSigners() is used to show sealer accounts and that the new node is added
        * clique.discard("new node address") once it is added, you discard the proposal 

    


