# daml-quick-start



# Concepts


1. Signatory(ies) (of template) and Controller(s) (of choices) provide authorization
    1. Signatories can be used as the authorization of a choice: This allows you to keep passing the original signatory into the sub-contracts of the business flow.  Example: ProductDefinition > ProductDefInstanceCreator > ProductInstance.  Where instanceCreator is the "role" contract that provides the user the ability to create product instances.

1. ensure cannot have actions like "fetch" because fetch causes an update:  Ensure was designed to "always be true" during the life of the contract so you cannot use other contracts as part of the ensure logic.

1. differences between `let` and `<-`: where the array is used as a computational builder in haskel.

1. Reading the Stadnard Lib docs for DAML is fairly difficult as you have to understand the haskel data structues.  More examples for quick copy and paste would be helpful.  Such as how to use queryContractId or queryContractKey.

1. 