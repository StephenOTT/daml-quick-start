# daml-quick-start



# Links:

Namespacing your Daml Files: https://discuss.daml.com/t/managing-namespaces-in-daml/1984

`do` and `<-` arrows: what do they do and why: https://discuss.daml.com/t/if-you-are-not-100-sure-about-do-blocks-notation-and-return-you-should-read-this/968

How to get Choice Params when building a UI: https://discuss.daml.com/t/daml-ui-template-multiple-parameter-choices-dialog-generation/506

ENT Data Orchestration: https://discuss.daml.com/t/great-blog-post-daml-smartcontracts-for-enterprise-data-orchestration-and-analytics-1/537

When and where to add Attachments: https://discuss.daml.com/t/on-attachments-what-you-should-and-shouldnt-store-on-a-ledger/579




# Concepts


1. Signatory(ies) (of template) and Controller(s) (of choices) provide authorization
    1. Signatories can be used as the authorization of a choice: This allows you to keep passing the original signatory into the sub-contracts of the business flow.  Example: ProductDefinition > ProductDefInstanceCreator > ProductInstance.  Where instanceCreator is the "role" contract that provides the user the ability to create product instances.

1. ensure cannot have actions like "fetch" because fetch causes an update:  Ensure was designed to "always be true" during the life of the contract so you cannot use other contracts as part of the ensure logic.

1. differences between `let` and `<-`: where the array is used as a computational builder in haskel.

1. Reading the Stadnard Lib docs for DAML is fairly difficult as you have to understand the haskel data structues.  More examples for quick copy and paste would be helpful.  Such as how to use queryContractId or queryContractKey.

1. 
