# Expense Pool Examples



## Expense Pool with State

Templates that supports creation of nonconsuming Pool Invites for new participants.
This allows multiple invites to be sent without having to place the pool in an "invite" state as is the case with the [Digital Asset Expense Pool Example](https://github.com/digital-asset/ex-models/tree/ca6818c10923360f7158e03a4c50dabe156f5419/expense-pool)

```daml
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


```daml
template Pool 
    with
        poolId: Text
        owner: Party
        name: Text
    where
        signatory owner     
```