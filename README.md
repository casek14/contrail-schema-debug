# Debug code for contrail-schema service

Start of schema transformer:
```
1. Main function
  |
  --- initialize discovery client
   |
   --- initialize logger
    |
    --- clean message queue
     |
     --- create Zookeeper client = elect master, after election start schema_transformer

2. Run schema transformer
   |
   --- Initialize introspect
    |
    --- Update connection state
     |
     --- Connect to config API
      |
      --- Create schema transformer instance

3. Schema transformer init
   |
   --- Initialize AMPQ
    |
    --- Initialize Cassandra
     |
     --- ServiceChain init (step 4.)
      |
      --- Run reinit of the resources (step 5.)

4. ServiceChain init
   |
   --- Read all service chains from config database
    |
    --- Check the chains if they are valid or not, if not mark them to deletion 

5. Reinit   
   |
   --- Run reinit of routing instances = check if routing instance still needed , if not mark for deletion
    |
    --- Check ACL if belongs to VN or SG, if not delete it
     |
     --- Update security groups
      |
      --- Run reinit of Route targets = Go through all RT, if RT have back ref to routing instance or to logical router 
          keep this RT, else delete this RT if no back_ref exists
       |
       --- Free RT from database
        |
        --- Check if RI of the VN has flag for deletion and check they sould be deleted
         |
         --- Network policy reinit
          |
          --- Reinit Virtual Machine Interface Check for updating virtual machine interface properties
           |
           --- Reinit Instance IPs and Floating IPs
            |
            --- Reinit Service instances = check virtual machine back refs, try locate VM from back refs, if exists VM name to the Service instance back ref list
             |
             --- Reinit Routing Policy = get routing instance refs and add the routing instance under the Routing policy and set RP as ref for RI
              |
              --- Reinit route aggregates
               |
               --- Reinit of Port tuple
                |
                --- Reinit of BGPaaS
                 |
                 --- Reinit Route Tables
                  |
                  --- Evaluate each VN = check for changes in VN policys, acls, interfaces, service chains, delete old network connections,  
                   |
                   --- Process stale objects = Go through all SI, delete SI with delete flag and update route targets
```
