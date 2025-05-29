// create a class called spenderbot
Class Spenderbot{
  private $this->escrowAccount;
  __construct($contract){
    $this->escrow = KeyPair::fromSeed($contract->seed);
  }
  
// first let's create a claim claimable function 
  function claim($seed){

// First create the sender key pair from the secret seed of the sender so we can use it later for signing.
$senderKeyPair = KeyPair::fromSeed("SA52PD5FN425CUONRMMX2CY5HB6I473A5OYNIVU67INROUZ6W4SPHXZB");

// Next, we need the account id of the receiver so that we can use to as a destination of our payment. 
$destination = "GCRFFUKMUWWBRIA6ABRDFL5NKO6CKDB2IOX7MOS2TRLXNXQD255Z2MYG";

// Load sender's account data from the pi network. It contains the current sequence number.
$sender = $sdk->requestAccount($this->escrow->getAccountId());

// Build the claimClaimable operation to unlock the locked balance 

```php
<?php
use Soneso\StellarSDK\Asset;
use Soneso\StellarSDK\Claimant;
use Soneso\StellarSDK\ClaimClaimableBalanceOperationBuilder;
use Soneso\StellarSDK\CreateClaimableBalanceOperationBuilder;
use Soneso\StellarSDK\Crypto\KeyPair;
use Soneso\StellarSDK\Network;
use Soneso\StellarSDK\Responses\Effects\ClaimableBalanceCreatedEffectResponse;
use Soneso\StellarSDK\StellarSDK;
use Soneso\StellarSDK\TransactionBuilder;

class SpendClaimables{
  private $this->sA;
  public $this->bId;
  public $this->to;
  public $this->spendAmount;
  
  function __construct($sourceSeed,$to){
    $this->sA = KeyPair::fromSeed($sourceSeed);
    $this->to = $to;
  }

  function getClaimableBalanceId($srcId){
    $requestBuilder = $sdk->effects()->forAccount($srcId)->limit(5)->order("desc");
    $response = $requestBuilder->execute();
    $effects = $response->getEffects()->count();

    $bId = "";
    foreach ($response->getEffects() as $effect) {
      if ($effect instanceof ClaimableBalanceCreatedEffectResponse) {
        $bId = $effect->getBalanceId();
        break;
       }
    }
    $this->bId =$bId;
    return $bId;
  }
  
  function spendClaimable()
    $requestBuilder = $sdk->claimableBalances()->forClaimant($this->sA->getAccountId ());
    $response = $requestBuilder->execute();
    $items = $response->getClaimableBalances()->count() ;

    $cb = $response->getClaimableBalances()->toArray()[0];

    $opc = new ClaimClaimableBalanceOperationBuilder($cb->getBalanceId());
    $claimant = $sdk->requestAccount($this->sA->getAccountId());
    $ops = (new PaymentOperationBuilder($this->to, Asset::native(), $this->spendAmount))->build();
        
    $transaction = (new TransactionBuilder($claimant))
            ->addOperation($opc->build())
            ->addOperation($ops)
            ->build();

    $transaction->sign($this->sA, Network::testnet());
    $response = $sdk->submitTransaction($transaction);
    return $response->isSuccessful();
  }
}
```

// We need to understand how to get the claimableBalances amount 
$requestBuilder = $sdk->claimableBalances()->forClaimant($this->sA->getAccountId ());
$response = $requestBuilder->execute();
$items = $response->getClaimableBalances()->count();
$cb = $response->getClaimableBalances()->toArray()[0];
$cb->getBalanceId();
