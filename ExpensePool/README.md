# Expense Pool Examples


## Expense Pool Chain (roles)

Each choice is nonconsuming: it uses the layering of signatories on each "sub-contract" and the role-contract pattern to establish a party that is allowed to read the original pool contract.  The Party is used as a role that the IAM would provide within the "readAs" prop of the user's permissions.  Part of Multi-Party Submissions feature of daml.


## Expense Pool with State

Templates that supports creation of nonconsuming Pool Invites for new participants.
This allows multiple invites to be sent without having to place the pool in an "invite" state as is the case with the [Digital Asset Expense Pool Example](https://github.com/digital-asset/ex-models/tree/ca6818c10923360f7158e03a4c50dabe156f5419/expense-pool)

This file contains a end to end test script.

```haskell
...
template Pool 
    with
        poolId: Text
        owner: Party
        name: Text
        participants: Set Party
        invitedParticipants: Set Party
    where
        signatory owner
        observer participants, invitedParticipants
...
```

## Expense Pool without State

Templates that supports creation of nonconsuming Pool Invites for new participants but the participants are not stored in the pool.  Instead the participants are stored as standalone contracts.

This allows multiple invites to be sent without having to place the pool in an "invite" state as is the case with the [Digital Asset Expense Pool Example](https://github.com/digital-asset/ex-models/tree/ca6818c10923360f7158e03a4c50dabe156f5419/expense-pool)

This allows the pool to remain more stable without having to update everytime a participant or invite change occurs.

TODO: How can the Invite and Participant contracts be "restricted" so they can only be created under the proper circumstances.


```haskell
template Pool 
    with
        poolId: Text
        owner: Party
        name: Text
    where
        signatory owner     
```



# Script Output

## Expense Pool with State

### Table

#### Pool
|id                                                                   |status                                                               |poolId   |owner  |name |participants.map |invitedParticipants.map|ALICE|BOB|
|---------------------------------------------------------------------|---------------------------------------------------------------------|---------|-------|-----|-----------------|-----------------------|------|------|
|#0:0                                                                 |archived                                                             |pool-1234|'Alice'|Pool1|GenMap[]         |GenMap[]               |S     |-     |
|#1:1                                                                 |archived                                                             |pool-1234|'Alice'|Pool1|GenMap[]         |GenMap['Bob'->{}]      |S     |O     |
|#2:3                                                                 |archived                                                             |pool-1234|'Alice'|Pool1|GenMap[]         |GenMap[]               |S     |W     |
|#3:1                                                                 |archived                                                             |pool-1234|'Alice'|Pool1|GenMap[]         |GenMap['Bob'->{}]      |S     |O     |
|#4:2                                                                 |archived                                                             |pool-1234|'Alice'|Pool1|GenMap['Bob'->{}]|GenMap[]               |S     |O     |
|#5:1                                                                 |archived                                                             |pool-1234|'Alice'|Pool1|GenMap[]         |GenMap[]               |S     |W     |
|#6:1                                                                 |archived                                                             |pool-1234|'Alice'|Pool1|GenMap[]         |GenMap['Bob'->{}]      |S     |O     |
|#7:2                                                                 |active                                                               |pool-1234|'Alice'|Pool1|GenMap['Bob'->{}]|GenMap[]               |S     |O     |

#### PoolInvite

|id   |status  |poolKey._1|poolKey._2|newParticipant|ALICE|BOB|
|-----|--------|----------|----------|--------------|------|------|
|#1:2 |archived|'Alice'   |pool-1234 |'Bob'         |S     |O     |
|#3:2 |archived|'Alice'   |pool-1234 |'Bob'         |S     |O     |
|#6:2 |archived|'Alice'   |pool-1234 |'Bob'         |S     |O     |



### Transactions

```haskell
Transactions: 
  TX 0 1970-01-01T00:00:00Z (ExpensePoolWithState:127:22)
  #0:0
  │   consumed by: #1:0
  │   referenced by #1:0
  │   known to (since): 'Alice' (0)
  └─> create ExpensePoolWithState:Pool
      with
        poolId = "pool-1234";
        owner = 'Alice';
        name = "Pool1";
        participants =
          (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
             map = GenMap[]);
        invitedParticipants =
          (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
             map = GenMap[])
  
  TX 1 1970-01-01T00:00:00Z (ExpensePoolWithState:137:38)
  #1:0
  │   known to (since): 'Alice' (1)
  └─> 'Alice' exercises InviteNewParticipant on #0:0 (ExpensePoolWithState:Pool)
              with
                newParticipant = 'Bob'
      children:
      #1:1
      │   consumed by: #2:0
      │   referenced by #2:0
      │   known to (since): 'Alice' (1), 'Bob' (1)
      └─> create ExpensePoolWithState:Pool
          with
            poolId = "pool-1234";
            owner = 'Alice';
            name = "Pool1";
            participants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap[]);
            invitedParticipants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap['Bob'->{}])
      
      #1:2
      │   consumed by: #2:2
      │   referenced by #2:1, #2:2
      │   known to (since): 'Alice' (1), 'Bob' (1)
      └─> create ExpensePoolWithState:PoolInvite
          with
            poolKey =
              (DA.Types:Tuple2@40f452260bef3f29dede136108fc08a88d5a5250310281067087da6f0baddff7 with
                 _1 = 'Alice'; _2 = "pool-1234");
            newParticipant = 'Bob'
  
  TX 2 1970-01-01T00:00:00Z (ExpensePoolWithState:146:22)
  #2:0
  │   known to (since): 'Alice' (2), 'Bob' (2)
  └─> 'Alice' exercises RemoveInvitedParticipant on #1:1 (ExpensePoolWithState:Pool)
              with
                participantToRemove = 'Bob'
      children:
      #2:1
      │   known to (since): 'Alice' (2), 'Bob' (2)
      └─> fetch #1:2 (ExpensePoolWithState:PoolInvite)
      
      #2:2
      │   known to (since): 'Alice' (2), 'Bob' (2)
      └─> 'Alice' exercises Archive on #1:2 (ExpensePoolWithState:PoolInvite)
                  with
      
      #2:3
      │   consumed by: #3:0
      │   referenced by #3:0
      │   known to (since): 'Alice' (2), 'Bob' (2)
      └─> create ExpensePoolWithState:Pool
          with
            poolId = "pool-1234";
            owner = 'Alice';
            name = "Pool1";
            participants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap[]);
            invitedParticipants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap[])
  
  TX 3 1970-01-01T00:00:00Z (ExpensePoolWithState:155:38)
  #3:0
  │   known to (since): 'Alice' (3)
  └─> 'Alice' exercises InviteNewParticipant on #2:3 (ExpensePoolWithState:Pool)
              with
                newParticipant = 'Bob'
      children:
      #3:1
      │   consumed by: #4:1
      │   referenced by #4:1
      │   known to (since): 'Alice' (3), 'Bob' (3)
      └─> create ExpensePoolWithState:Pool
          with
            poolId = "pool-1234";
            owner = 'Alice';
            name = "Pool1";
            participants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap[]);
            invitedParticipants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap['Bob'->{}])
      
      #3:2
      │   consumed by: #4:0
      │   referenced by #4:0
      │   known to (since): 'Alice' (3), 'Bob' (3)
      └─> create ExpensePoolWithState:PoolInvite
          with
            poolKey =
              (DA.Types:Tuple2@40f452260bef3f29dede136108fc08a88d5a5250310281067087da6f0baddff7 with
                 _1 = 'Alice'; _2 = "pool-1234");
            newParticipant = 'Bob'
  
  TX 4 1970-01-01T00:00:00Z (ExpensePoolWithState:164:24)
  #4:0
  │   known to (since): 'Alice' (4), 'Bob' (4)
  └─> 'Bob' exercises AcceptInvite on #3:2 (ExpensePoolWithState:PoolInvite)
            with
      children:
      #4:1
      │   known to (since): 'Alice' (4), 'Bob' (4)
      └─> 'Bob' exercises InviteAccept on #3:1 (ExpensePoolWithState:Pool)
                with
                  invite =
                    (ExpensePoolWithState:PoolInvite with
                       poolKey =
                         (DA.Types:Tuple2@40f452260bef3f29dede136108fc08a88d5a5250310281067087da6f0baddff7 with
                            _1 = 'Alice'; _2 = "pool-1234");
                       newParticipant = 'Bob')
          children:
          #4:2
          │   consumed by: #5:0
          │   referenced by #5:0
          │   known to (since): 'Alice' (4), 'Bob' (4)
          └─> create ExpensePoolWithState:Pool
              with
                poolId = "pool-1234";
                owner = 'Alice';
                name = "Pool1";
                participants =
                  (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                     map = GenMap['Bob'->{}]);
                invitedParticipants =
                  (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                     map = GenMap[])
  
  TX 5 1970-01-01T00:00:00Z (ExpensePoolWithState:173:22)
  #5:0
  │   known to (since): 'Alice' (5), 'Bob' (5)
  └─> 'Alice' exercises RemoveParticipant on #4:2 (ExpensePoolWithState:Pool)
              with
                participantToRemove = 'Bob'
      children:
      #5:1
      │   consumed by: #6:0
      │   referenced by #6:0
      │   known to (since): 'Alice' (5), 'Bob' (5)
      └─> create ExpensePoolWithState:Pool
          with
            poolId = "pool-1234";
            owner = 'Alice';
            name = "Pool1";
            participants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap[]);
            invitedParticipants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap[])
  
  TX 6 1970-01-01T00:00:00Z (ExpensePoolWithState:182:38)
  #6:0
  │   known to (since): 'Alice' (6)
  └─> 'Alice' exercises InviteNewParticipant on #5:1 (ExpensePoolWithState:Pool)
              with
                newParticipant = 'Bob'
      children:
      #6:1
      │   consumed by: #7:1
      │   referenced by #7:1
      │   known to (since): 'Alice' (6), 'Bob' (6)
      └─> create ExpensePoolWithState:Pool
          with
            poolId = "pool-1234";
            owner = 'Alice';
            name = "Pool1";
            participants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap[]);
            invitedParticipants =
              (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                 map = GenMap['Bob'->{}])
      
      #6:2
      │   consumed by: #7:0
      │   referenced by #7:0
      │   known to (since): 'Alice' (6), 'Bob' (6)
      └─> create ExpensePoolWithState:PoolInvite
          with
            poolKey =
              (DA.Types:Tuple2@40f452260bef3f29dede136108fc08a88d5a5250310281067087da6f0baddff7 with
                 _1 = 'Alice'; _2 = "pool-1234");
            newParticipant = 'Bob'
  
  TX 7 1970-01-01T00:00:00Z (ExpensePoolWithState:191:24)
  #7:0
  │   known to (since): 'Alice' (7), 'Bob' (7)
  └─> 'Bob' exercises AcceptInvite on #6:2 (ExpensePoolWithState:PoolInvite)
            with
      children:
      #7:1
      │   known to (since): 'Alice' (7), 'Bob' (7)
      └─> 'Bob' exercises InviteAccept on #6:1 (ExpensePoolWithState:Pool)
                with
                  invite =
                    (ExpensePoolWithState:PoolInvite with
                       poolKey =
                         (DA.Types:Tuple2@40f452260bef3f29dede136108fc08a88d5a5250310281067087da6f0baddff7 with
                            _1 = 'Alice'; _2 = "pool-1234");
                       newParticipant = 'Bob')
          children:
          #7:2
          │   known to (since): 'Alice' (7), 'Bob' (7)
          └─> create ExpensePoolWithState:Pool
              with
                poolId = "pool-1234";
                owner = 'Alice';
                name = "Pool1";
                participants =
                  (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                     map = GenMap['Bob'->{}]);
                invitedParticipants =
                  (DA.Set.Types:Set@97b883cd8a2b7f49f90d5d39c981cf6e110cf1f1c64427a28a6d58ec88c43657 with
                     map = GenMap[])

Active contracts:  #7:2

Return value: {}
```