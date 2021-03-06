### How to create a Mosaic

```typescript
import {
    NEMLibrary, NetworkTypes, TimeWindow, Account, TransactionHttp,
    MosaicDefinitionCreationTransaction, MosaicDefinition, PublicAccount, MosaicId, MosaicProperties, MosaicLevy,
    MosaicLevyType
} from "nem-library";

declare let process: any;

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const privateKey: string = process.env.PRIVATE_KEY;
const account = Account.createWithPrivateKey(privateKey);
const transactionHttp = new TransactionHttp();

const mosaicDefinitionTransaction = MosaicDefinitionCreationTransaction.create(
    TimeWindow.createWithDeadline(),
    new MosaicDefinition(
        PublicAccount.createWithPublicKey(account.publicKey),
        new MosaicId("new-namespace", "new-mosaic"),
        "mosaic description",
        new MosaicProperties(0, 9000000, true, true),
        new MosaicLevy(
            MosaicLevyType.Percentil,
            account.address,
            new MosaicId("nem", "xem"),
            2
        )
    )
);

const signedTransaction = account.signTransaction(mosaicDefinitionTransaction);
transactionHttp.announceTransaction(signedTransaction).subscribe( x => console.log(x));

```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/mosaic/How_to_create_a_Mosaic.ts)


### How to get Namespace Mosaic Definitions
```typescript
import {
    NEMLibrary, NetworkTypes, MosaicHttp, TransactionTypes
} from "nem-library";

// Initialize NEMLibrary for TEST_NET Network
NEMLibrary.bootstrap(NetworkTypes.TEST_NET);

const mosaicHttp = new MosaicHttp();
const namespace = "new-namespace";

mosaicHttp.getAllMosaicsGivenNamespace(namespace).subscribe(mosaicDefinitions => console.log(mosaicDefinitions));
```

[Source code](https://github.com/aleixmorgadas/nem-library-examples/blob/master/howto/mosaic/How_to_get_Namespace_Mosaic_Definitions.ts)

