```php
namespace Ecommerce\Domain\Models\Orders\Order;
use Illuminate\Database\Eloquent\Model;
use Ecommerce\Domain\Models\{Payment\PaymentId, Shipping\ShippingId, Cart\CartId, Billing\BillingId};

protected float $total=0.00;

protected Shopperld $shopper;
protected CartId $cartId;
protected ShippingId $shippingId;
protected PaymentId $paymentId;
protected $fillable = ['shopper_id', 'cart_id', 'payment_id', 'shipping_id'];

public function __construct(Shopper $shopper, CartId $cartId, Payment$paymentId=null, ShippingId $shippingId=null)
{
	parent::__construct();
	$this->shopperId = $shopperld;
	$this->cartId = $cartId;
	$this->billingId = $billingId;
	$this->shippingId = $shippingId;
}
	public function orderLines()
	{
		return $this->hasMany(OrderLine::class);
	}
}
```

```php
namespace Ecommerce\Domain\Models\Orders\Order;

use Illuminate\Database\Eloquent\Model;
use Ecommerce\Domain\Models\{Product\ProductId, Order\OrderId);

class OrderLine extends Model
{
  protected Order $order;	
  protected Product $product;
  protected int $quantity;

  protected $fillable = ['product_id', 'orderLineAmount', 'order_id', 'quantity'];

	public function __construct(Product $product, int $quantity, Order $order)	{
		parent::construct();
		$this->product = $product;
		$this->quantity = $quantity;
	}
	public function order()
	{
		return $this->belongsTo(Order::class);
	}
	public function product()
	{
		return $this->hasOne(Product::class);
	}
}
```

```php
//create order object
$order = new Order($shopperId, $cartld);
//create product object & quantity of that product
$product = Product::find(420);
$quantity = 3;
$orderLine = OrderLine::create($product, $quantity);
$order->orderLine->associate($orderLine);
$order->save();
```

```php
$order = new Order($shopperId, $cartId);
$order->addOrderLine($product, $qty);
```


```php
//use cases & namespaces
use Ecommerce\Domain\Models\Orders\OrderStatus;use Illuminate\Support\Facades\DB;
class Order extends Model{
	//methods and property definitions	public $orderLines = [];	const TAXRATE = .10;
	private $status = OrderStatus::ORDER_STARTED;
	/*
	* Invariant #2 & #3 are protected here
	*/
	public function addOrderLine(Product $product, int $qty=1)	{
		$orderLine = new OrderLine($product, $qty);
		$price = $product->price;
		foreach ($qty as $q) {
			$this->total += ($price +
				(static::TAX_RATE * $price));
		}
		$this->orderLines[] = $orderLine;
	}
	/**
	* Invariant #1 is protected here
	*/
	public function startCheckout()
	{
	if (!empty($this->orderLines) &&
		(count($this->orderLines) > 0)) {
		//save the order lines within a transaction so we can		//guarantee the state of the order stays consistent		DB::transaction(function() {
			foreach ($this->orderLines as $orderLine) {
        $this->associate($orderLine);
			}
			$this->save();
		});
		/*start checkout process with a job or service.
		In theory, this would also change the status of the
		Order to something like OrderStatus:CHECKOUT_STARTED*/
		}	
		return new JsonResponse("Order must have at least one Order		Line before Checkout can begin", 500);
	}
}
```

```php
//create order object
$order = new Order($shopperId, $cartId);
//create product object & quantity of that product
$product = Product::find(420);
$quantity = 3;

//we no longer have to worry about the orderLine object at all!
//instead, we just pass in the data we already have...
$order->addOrderLine($product, $qty);
$order->addOrderLine($product2, $qty2);
$order->addOrderLine($product3, $qty3);

//user clicks on the "Checkout" button
if ($order->startCheckout()) {
  dispatch(new RunCheckout($order));
} else {
  //return some response indicating to the frontend the issue
  //which would presumably display a notification to the user
}
```

```php
//namespace & use statements
class Order extends Model
{
    //properties and method definitions
    /**
    * @param Ssequence : The location of the order line in the array
    */
    public function removeOrderLine($sequence)
    {
        if (isset($this->orderLines[$sequence])) {
            unset($this->orderLines[$sequence]));
        }
    }
    
    public function updateOuantity($sequence, $newOuantity)
    {
        if (isset($this->orderLines[$sequence])) {
            //get the product that corresponds to that order line:
            $product = $this->orderLines[$sequence]->product;
            //remove the orderLine completely from the array:
            unset($this->orderLines[$sequence]);
            //add the new orderLine to the array:
            $this->addOrderLine($product, $newOuantity);
        }
    }
}
```
