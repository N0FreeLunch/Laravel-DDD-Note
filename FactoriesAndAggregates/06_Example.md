
## Order 모델
```php
namespace Ecommerce\Domain\Models\Orders\Order;

use Illuminate\Database\Eloquent\Model;
use Ecommerce\Domain\Models\{Payment\PaymentId, Shipping\ShippingId, Cart\CartId, Billing\BillingId};

class Order extends Model
{
    protected float $total=0.00;

    protected Shopperld $shopper;
    protected CartId $cartId;
    protected ShippingId $shippingId;
    protected PaymentId $paymentId;
    protected $fillable = ['shopper_id', 'cart_id', 'payment_id', 'shipping_id'];

    public function __construct(Shopper $shopper, CartId $cartId, Payment $paymentId=null, ShippingId $shippingId=null)
    {
        parent::__construct();
        $this->shopperId = $shopperId;
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
- Order모델과 OrderLine 모델의 관계는 1:n 관계
- 하나의 주문은 여러 OrderLine을 가져야 한다. 왜냐하면 주문라인은 상황에 따라서 여러 다른 총액 상황을 가지고 있다.
- 상황에 따른 주문 총액 계산이 달라지기 때문에 이를 기록해야 해서 하나의 Order 모델에는 여러 OrderLine을 가져야 하는 것.
- 클레스의 멤버 변수의 값은 엔티티에 해당한다. 
- 일반적으로 객체의 멤버 변수를 세팅할 때, 생성자를 통해서 세팅한다는 것은 한 번만 세팅해서 값을 가져다 쓰겠다는 의미이다.
- 함수의 파라메터에서 `$paymentId=null, ShippingId $shippingId=null` 는 빈 값으로 입력 될 수 있다.

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

    public function __construct(Product $product, int $quantity, Order $order)
    {
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

```
	public function product()
	{
		return $this->hasOne(Product::class);
	}
```
- OrderLine 클래스는 내부에는 Product model을 릴레이션으로 정의하고 있다. 

```
	public function __construct(Product $product, int $quantity, Order $order)	{
		parent::construct();
		$this->product = $product;
		$this->quantity = $quantity;
	}
```
- OrderLine 클래스는 내부에 특정 상품에 대한 금액 합계와 수량에 관한 정보가 들어 있다.


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
- `$order->orderLine->associate($orderLine);` 애그리게이트 객체를 생성해야 하는데 주문 객체에서 주문라인 객체로 접근하는 방식으로 접근하는 방식이다.

```php
//use cases & namespaces
use Ecommerce\Domain\Models\Orders\OrderStatus;
use Illuminate\Support\Facades\DB;

class Order extends Model
{
    //methods and property definitions    
    public $orderLines = [];
    const TAXRATE = .10;
    private $status = OrderStatus::ORDER_STARTED;
    /*
    * Invariant #2 & #3 are protected here
    */
    public function addOrderLine(Product $product, int $qty=1)
    {
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
            //save the order lines within a transaction so we can        //guarantee the state of the order stays consistent        DB::transaction(function() {
            foreach ($this->orderLines as $orderLine) {
                $this->associate($orderLine);
            }
            $this->save();
        });
        /*start checkout process with a job or service.
        In theory, this would also change the status of the
        Order to something like OrderStatus:CHECKOUT_STARTED*/
        }    
        return new JsonResponse("Order must have at least one Order        Line before Checkout can begin", 500);
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

```php
//namespace & use statements
class Order extends Model
{
    //properties and method definitions
    /**
    * @param $sequence : The location of the order line in the array
    */
    public function removeOrderline($sequence)
    {
        if (isset($this->orderLines[$sequence])) {
            $orderLine = $this->orderLines[$sequence];
            $totalAmountDelta = $orderLine->product->price +
				($orderLine->product->price * static::TAX_RATE);
            $this->total -= $totalAmountDelta;
            unset($this->orderLines[$sequence]));
        }
    }
		
    public function updateQuantity($sequence, $newOuantity)
    {
        if (isset($this->orderLines[$sequence])) {
            //get the product that corresponds to that order line:
            $orderLine = $this->orderLines[$sequence];
            $product = $orderLine->product;
            //remove the orderLine completely from the array:
            $totalAmountDelta = $product->price + ($product->price
				* static::TAX_RATE);
            $this->amount -= $totalAmountDelta;
            unset($this->orderLines[$sequence]);
            //we dont have to worry about adding the product's
            //tax because that logic is already in addOrderLine():
            $this->addOrderLine($product, SnewOuantity);
        }
    }
}
```
