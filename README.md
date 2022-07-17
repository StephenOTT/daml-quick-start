# daml-quick-start



# Links:

Namespacing your daml Files: https://discuss.daml.com/t/managing-namespaces-in-daml/1984

`do` and `<-` arrows: what do they do and why: https://discuss.daml.com/t/if-you-are-not-100-sure-about-do-blocks-notation-and-return-you-should-read-this/968

How to get Choice Params when building a UI: https://discuss.daml.com/t/daml-ui-template-multiple-parameter-choices-dialog-generation/506

ENT Data Orchestration: https://discuss.daml.com/t/great-blog-post-daml-smartcontracts-for-enterprise-data-orchestration-and-analytics-1/537

When and where to add Attachments: https://discuss.daml.com/t/on-attachments-what-you-should-and-shouldnt-store-on-a-ledger/579




# Concepts

1. https://docs.daml.com/daml/intro/10_Functional101.html

1. When adding some deps like adding Daml.Trigger for the first time in VS Code, you have to restart VS code or else you get a build error in VS code.

1. Signatory(ies) (of template) and Controller(s) (of choices) provide authorization
    1. Signatories can be used as the authorization of a choice: This allows you to keep passing the original signatory into the sub-contracts of the business flow.  Example: ProductDefinition > ProductDefInstanceCreator > ProductInstance.  Where instanceCreator is the "role" contract that provides the user the ability to create product instances.

1. ensure cannot have actions like "fetch" because fetch causes an update:  Ensure was designed to "always be true" during the life of the contract so you cannot use other contracts as part of the ensure logic.

1. differences between `let` and `<-`: where the array is used as a computational builder in haskel.

1. Reading the Standard Lib docs for DAML is fairly difficult as you have to understand the haskel data structures.  More examples for quick copy and paste would be helpful.  Such as how to use queryContractId or queryContractKey.

1. **Assess your potential for Contract Contention:** What are the chances that there are multiple users trying to action choices on a single contract in parallel/at the same time?  If you have high likelihood of contention and strongly consider using a distributed contract approach.  If you have very low likelihood of contention then using a master-contract/aggregate contract approach is likely much less effort to build, maintain, and understand.
   1. Having more contracts means distribution, less or no contention, many more active contracts, and smaller data size per contract.  Having a single contract means larger data size, less active contracts (more contracts you can prune over time and keep ledger smaller), and higher chance of contention (when applicable).


1. Upgrading an Application means creating new versions of contracts:
   1. see the docs on upgrading.  There can be large implications when dealing with large number of contracts
   1. Contract extensions are really just creating more contract types / templates that reference the older version.  An extension is still tied to a version so will have some version management thoughts.
   1. You can batch contract upgrades by collecting a list of contract IDs.


1. active contract set (ACS)


# Helpers


## Code Comments

```haskel
-- this is a single line comment

{-
this is a
multi-line comment
-}
```

## Importing

1. import example
1. import a template from another module `import MyModule (MyTemplate)`
1. import only a specific function name `import DA.List (someFuncName)`


# fst and snd functions use with Map and common queries that return a truple (arg1, arg2)

https://docs.daml.com/daml/stdlib/Prelude.html#function-da-internal-prelude-fst-55341


# Graph Structure

TransactionTree:

1. Transaction Id to one or more Event IDs
1. WorkflowId one or more Transaction Ids
1. CommandId to one or more Transaction Ids.
1. Event Ids:
   1. Parties: WitnessParties, signatories, observers, actingParties
1. TemplateId > TransactionIds
1. Exercise Contract Id > Exercise Result
1. EventId > ChildEventIds (multiple possible)
1. If event is consuming
1. 


## Group By:

1. Transaction Id
1. CommandId
1. WorkflowId
1. EffectiveAt Date, Hour, etc
1. Signatories
1. Observers
1. Witnesses
1. TemplateId
1. Acting Parties
1. Create vs Exercise
1. Exercise Result Type
