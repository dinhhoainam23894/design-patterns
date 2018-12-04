![Design Patterns For Humans](https://cloud.githubusercontent.com/assets/11269635/23065273/1b7e5938-f515-11e6-8dd3-d0d58de6bb9a.png)

***

<p align="center">
🎉 Giải thích cực kỳ đơn giản về design patterns! 🎉
</p>
<p align="center">
Một chủ đề có thể dễ dàng làm cho tâm trí của mọi người lung lay. Ở đây tôi cố gắng làm cho chúng gần gũi với cách suy nghĩ của bạn (và có thể là của tôi) bằng cách giải thích chúng theo cách <i>đơn giản nhất</i> có thể.
</p>

***

<sub>Vào xem [blog] của tôi(http://kamranahmed.info) và "chào" tôi trên [Twitter](https://twitter.com/kamranahmedse).</sub>

Giới thiệu
=================

Design patterns là giải pháp cho các vấn đề thường gặp; **hướng dẫn cách giải quyết các vấn đề nhất định**. Chúng không phải các class, package hay là các thư viện mà bạn có thể thêm nó vào ứng dụng của bạn và chờ điều kỳ diệu xảy ra. Đây là những hướng dẫn về cách giải quyết các vấn đề nhất định trong các tình huống nhất định.

> Design patterns là các giải pháp cho các vấn đề thường gặp; hướng dẫn cách giải quyết các vấn đề nhất định.

Wikipedia mô tả chúng như

> Trong kỹ thuật phần mềm, design pattern một phần mềm là một giải pháp tái sử dụng chung cho một vấn đề thường xảy ra trong một bối cảnh nhất định trong thiết kế phần mềm. Nó không phải là một thiết kế hoàn chỉnh có thể được chuyển đổi trực tiếp thành source hoặc machine code. Nó là một mô tả hoặc mẫu để làm thế nào để giải quyết một vấn đề có thể được sử dụng trong nhiều tình huống khác nhau.

⚠️ Hãy cẩn thận
-----------------
- Design patterns không phải là một viên đạn bạc(giải quyết được) cho tất cả các vấn đề của bạn.
- Đừng cố ép buộc phải tuân theo chúng ; có thể có điều xấu xảy ra, nếu làm như vậy. 
- Hãy nhớ rằng  design patterns là giải pháp **giải quyết** các vấn đề, không phải là giải pháp **tìm** các vấn đề; không nên quá suy nghĩ về nó.
- Nếu được sử dụng đúng chỗ một cách chính xác, nó có thể là một vị cứu tinh; hoặc ngược lại nó có thể dẫn đến một mớ hỗn độn kinh khủng trong code.

> Lưu ý là các ví dụ dưới đây là của phiên bản PHP-7, Tuy nhiên điều này cũng có thể đúng với các phiên bản khác vì khái niệm của nó giống nhau.

Các loại Design Patterns
-----------------

* [Creational](#creational-design-patterns)
* [Structural](#structural-design-patterns)
* [Behavioral](#behavioral-design-patterns)

Creational Design Patterns
==========================

Nói một cách đơn giản
> Creational patterns are được tập trung hướng tới cách khởi tạo một đối tượng hoặc một nhóm đối tượng liên quan.

Theo Wikipedia :
> Trong kỹ thuật phần mềm , creational design patterns là mẫu thiết kế đối phó với các cơ chế tạo đối tượng, cố tạo đối tượng theo cách phù hợp với tình huống. Hình thức tạo đối tượng cơ bản có thể dẫn đến các vấn đề về thiết kế hoặc thêm độ phức tạp vào thiết kế. Creational design patterns giải quyết vấn đề này bằng cách nào đó kiểm soát việc tạo đối tượng này.

 * [Simple Factory](#-simple-factory)
 * [Factory Method](#-factory-method)
 * [Abstract Factory](#-abstract-factory)
 * [Builder](#-builder)
 * [Prototype](#-prototype)
 * [Singleton](#-singleton)

🏠 Simple Factory
--------------
Ví dụ thực tế : 
> Hãy xem xét, bạn đang xây dựng một ngôi nhà và bạn cần cửa ra vào. Bạn có thể mặc quần áo thợ mộc, mang một ít gỗ, keo, đinh và tất cả các dụng cụ cần thiết để xây cửa và bắt đầu xây dựng nó trong nhà hoặc bạn chỉ cần gọi nhà máy và nhận cửa được làm xong cho bạn để bạn không cần phải tìm hiểu bất cứ điều gì về việc làm cửa hoặc để đối phó với mớ hỗn độn mà đi kèm với việc làm ra nó..

Nói một cách đơn giản
> Simple factory đơn giản là tạo ra một instance cho client mà không làm lộ ra bất kỳ instantiation logic cho client

Theo Wikipedia :
> Trong lập trình hướng đối tượng (OOP), một factory là một đối tượng để tạo các đối tượng khác – một factory là một hàm hoặc phương thức trả về các đối tượng của một nguyên mẫu hoặc class khác nhau từ một số lời gọi phương thức, được giả định là "new".

**Ví dụ về lập trình**

Đầu tiên chúng ta có một interface Door và implementation
```php
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```
Su đó chúng ta có một nhà máy sản xuất cửa (DoorFactory) để tạo ra cửa(makeDoor) và trả về nó
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```
Và sau đó nó có thể được sử dụng như sau :
```php
// Make me a door of 100x200
$door = DoorFactory::makeDoor(100, 200);

echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();

// Make me a door of 50x100
$door2 = DoorFactory::makeDoor(50, 100);
```

**Khi nào thì sử dụng nó?**

Khi tạo một đối tượng không chỉ là một vài nhiệm vụ và liên quan đến một số logic, nó có ý nghĩa để đặt nó trong một factory thay vì lặp lại cùng một đoạn code ở khắp mọi nơi.
.

🏭 Factory Method
--------------

Ví dụ thực tế :
> Hãy xem xét trường hợp của một người quản lý tuyển dụng. Một người không thể phỏng vấn cho từng vị trí. Dựa trên việc tuyển nhân viên, cô ấy phải quyết định và ủy nhiệm các bước phỏng vấn cho những người khác nhau.


Nói một cách đơn giản
> Nó cung cấp một cách để ủy quyền logic instantiation cho các class con.

Theo Wikipedia :
> Trong class - cơ sở của lập trình, factory method pattern là một creational pattern mà sử dụng các method factory để giải quyết vấn đề về khởi tạo các object mà không cần xác định chính xác class của object mà sẽ được tạo ra. Điều này được thực hiện bằng cách tạo ra những object thông qua việc gọi một method factory - hoặc được chỉ định trong interface và implement bởi các class con, hoặc được implement trong class base và ghi đè tùy ý bởi các class dẫn xuất - thay vì được gọi thông qua hàm khởi tạo.

 **Ví dụ về lập trình**

Lấy ví dụ về người quản lý tuyển dụng của chúng ta ở trên. Đầu tiên chúng ta có một interface interviewer và một số cái implementations nó

```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about design patterns!';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about community building';
    }
}
```

Bây giờ chúng ta hãy tạo `HiringManager`

```php
abstract class HiringManager
{

    // Factory method
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```
Bây giờ bất kỳ class con nào cũng có thể mở rộng và cung cấp theo yêu cầu của interviewer
```php
class DevelopmentManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```
và sau đó nó có thể được sử dụng như :

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Output: Asking about design patterns

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Output: Asking about community building.
```

**Khi nào thì sử dụng nó?**

Nó hữu ích khi có một số việc được xử lý chung trong một class nhưng các class con được yêu cầu có thể được đưa ra bởi các quyết định linh động trong khi chạy. Hay nói cách khác, khi client không biết chính xác class con nào là cần thiết.

🔨 Abstract Factory
----------------

Ví dụ thực tế
> Mở rộng ví dụ về cửa ở trên Simple Factory. Dựa trên việc bạn cần là lấy một chiếc cửa gỗ từ cửa hàng cửa gỗ, cửa sắt từ cửa hàng sắt hoặc cửa nhựa từ một cửa hàng liên quan. thêm vào đó là bạn cần những người với các đặc điểm khác nhau để phù hợp với cái cửa đó, ví dụ như bạn cần một thợ mộc cho chiếc cửa gỗ, thợ hàn cho chiếc cửa sắt,... Và giờ bạn đã thấy sự phụ thuộc khác nhau giữa những chiếc cửa, cửa gỗ cần thợ mộc, cửa sắt cần thợ hàn,..

Nói một cách đơn giản
> Một factory của các factory; một factory nhóm những cá thể nhưng các factory liên kết/phụ thuộc lẫn nhau mà không cần chỉ rõ các class cụ thể của nó.

Theo Wikipedia:
> The abstract factory pattern cung cấp một cách để gói gọn một nhóm các factory riêng lẻ có một chủ đề chung mà không cần chỉ định các class cụ thể của nó
**Ví dụ về lập trình**

Theoc ví dụ về cửa ở trên. Đầu tiên chúng ta có `Door` interface và một số cái implementation nó

```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo 'I am a wooden door';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo 'I am an iron door';
    }
}
```
Sau đó, chúng tôi có một số chuyên gia phù hợp cho từng loại cửa

```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit iron doors';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'I can only fit wooden doors';
    }
}
```

Bây giờ chúng ta có abstract factory sẽ cho phép chúng ta tạo ra một nhóm các object có liên quan tới nhau. ví dụ như nhà máy cửa gỗ sẽ tạo ra cửa gỗ và chuyên gia phù hợp với cửa gỗ, và nhà máy cửa sắt tạo ta cửa sắt và chuyên gia phù hợp với cửa sắt.
```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// Wooden factory to return carpenter and wooden door
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// Iron door factory to get iron door and the relevant fitting expert
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```
Và sau đó nó có thể được sử dụng như sau :
```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // Output: I am a wooden door
$expert->getDescription(); // Output: I can only fit wooden doors

// Same for Iron Factory
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // Output: I am an iron door
$expert->getDescription(); // Output: I can only fit iron doors
```

Như bạn có thể thấy thì một nhà máy cửa gỗ được đóng gói cả `thợ mộc` và `cửa gỗ` cũng như nhà máy cửa sắp đóng gói cả `cửa sắt` và `thợ hàn `. Và do đó nó đảm bảo với chúng ta là với mỗi cánh cửa được tạo ra, chúng ta sẽ không lấy nhầm một chuyên gia.
**Khi nào thì sử dụng nó?**

Khi có sự tương quan giữa phụ thuộc và các logic khởi tạo liên quan không đơn giản

👷 Builder
--------------------------------------------
Ví dụ thực tế
> Hãy tưởng tượng là bạn đang ở Hardee's và bạn đặt một đơn hàng đặc biệt, hãy nói "Big hardee" và họ đưa cho bạn mà không có *bất kì câu hỏi* nào; đây là một ví dụ về simple factory. Nhưng đâu là những trường hợp khi logic khởi tạo liên quan tới nhiều bước. Ví dụ như bạn muốn tùy chỉnh đơn Subway, bạn có nhiều lựa chọn trong việc chiếc burger của bạn được làm như nào như bạn đang muốn bánh mì gì? loại sốt mà bạn muốn?... Trong những trường hợp như vậy, builder pattern được sử dụng như một giải pháp.

Nói một cách đơn giản
> Cho phép bạn bạn tạo các đặc điểm khác nhau của object trong khi tránh bị ảnh hưởng việc khởi tạo. Nó hữu dụng khi có thể tạo nhiều tùy chọn cho một object. Hoặc khi có quá nhiều bước trong việc tạo ra một object.

Theo Wikipedia : 
> Builder pattern là một object thuộc nhóm design pattern khởi tạo phần mềm với ý tưởng tìm kiếm giải pháp chống lại việc khởi tạo.

Hãy để tôi giới thiệu thêm một chút về mô hình chống lại việc khởi tạo này. Tại một thời điểm khác, chúng tôi đã thấy một constructor như dưới đây:

```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

Như bạn thấy, số lượng tham số của hàm khởi tạo có thể nhanh chóng làm bạn mất kiểm soát và nó dần trở nên rất khó hiểu về sự sắp xếp các tham số. Thêm nữa là danh sách những tham số có thể tiếp tục phát triển nếu bạn muốn thêm nhiều option trong tương lai. Điều này được gọi là mô hình chống lại việc khởi tạo..

**Ví dụ về lập trình**

Cách thay thế tốt nhất là sử dụng builder pattern. Trước heetschungs ta có một burger mà chúng ta muốn làm

```php
class Burger
{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```
Và sau đó chúng ta có một builder

```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```
Và sau đó nó có thể được sử dụng như sau :

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**Khi nào sử dụng nó?**

Khi có thể có một số đặc điểm của object và tránh việc chống lại khởi tạo. Sự khác biệt chính của factory pattern là đây; factory pattern được sử dụng khi việc khởi tạo chỉ có một bước trong tiến trình trong khi builder pattern được sử dụng khi có nhiều bước trong quá trình.

🐑 Prototype
------------
Ví dụ thực tế
> Bạn có nhớ dolly? Con cừu mà được nhân bản! Cho phép tôi không đi vàocacs thông tin chi tiết nhưng điểm mấu chốt ở đây là tất cả những thứ về nhân bản.

Nói một cách đơn giản
> Tạo đối tượng dựa trên đối tượng hiện có thông qua nhân bản.

Theo Wikipedia:
> Prototype pattern là một creational design pattern trong phát triển phần mềm. Nó được sử dụng khi kiểu của đối tượng cần tạo được định nghĩa bởi một thực thể nguyên mẫu, giống nhwu việc nhân bản nó để tạo ra một đối tượng mới.

Tóm lại, nó cho phép bạn tạo một bản sao của một đối tượng hiện có và sửa đổi nó theo nhu cầu của bạn, thay vì trải qua những rắc rối khi tạo một đối tượng từ đầu và thiết lập nó.

**Ví dụ về lập trình**

Trong PHP, Nó thực hiện đơn giản bằng cách sử dụng `clone`

```php
class Sheep
{
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = 'Mountain Sheep')
    {
        $this->name = $name;
        $this->category = $category;
    }

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function setCategory(string $category)
    {
        $this->category = $category;
    }

    public function getCategory()
    {
        return $this->category;
    }
}
```
Sau đó, nó có thể được nhân bản như sau :
```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // Mountain Sheep

// Clone and modify what is required
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // Mountain sheep
```

Ngoài ra bạn có thể sử dụng magic method `__clone` để sửa đổi hành vi nhân bản.

**Khi nào thì sử dụng?**

Khi một đối tượng được yêu cầu phải tương tự như đối tượng hiện có hoặc khi việc khởi tạo mất nhiều công hơn việc nhân bản..

💍 Singleton
------------
Ví dụ thực tế
> Cùng một lúc chỉ có thể có một tổng thống đối với mỗi quốc gia. Cùng một tổng thống phải đưa ra được hành động bất cứ khi nào có nhiệm vụ. Tổng thống ở đây là một singleton..

Nói một cách đơn giản
> Đảm bảo rằng chỉ có một đối tượng của một class cụ thể được tạo ra.

Theo Wikipedia
> Trong kĩ nghệ phần mềm,  singleton pattern là một mẫu thiết kế phần mềm hạn chế sự khởi tạo của một class thành một đối tượng. Điều này rất hữu ích khi cần một đối tượng chính xác để điều phối các hành động trên toàn hệ thống..

Singleton pattern thực sự được voi là anti-pattern và nên tránh lạm dụng nó. Nó không hoàn toàn là xấu và có thể có một số trường hợp sử dụng hợp lệ nhưng nên được sử dụng thận trọng vì nó giới thiệu một trạng thái toàn cầu trong ứng dụng của bạn và thay đổi nó ở một nơi có thể ảnh hưởng đến các khu vực khác và nó có thể trở nên khá khó khăn để debug. Điều tồi tệ khác về nó là nó làm cho code của bạn khó có thể bắt trước theo kiểu singleton.

**Ví dụ về lập trình**

Để tạo một singleton, Tạo constructor private, vô hiệu hóa nhân bản, vô hiệu hóa việc mở rộng và tạo các biến static chưứa instance
```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // Hide the constructor
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // Disable cloning
    }

    private function __wakeup()
    {
        // Disable unserialize
    }
}
```
Sau đó thì sử dụng như sau :
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```
Structural Design Patterns
==========================
Nói một cách đơn giản

> Structural pattern chủ yếu quan tâm tới các thành phần đối tượng hay nói cách khác là các thực thể có thể tương tác lẫn nhau như thế nào. Hoặc giải thích khác sẽ là, chúnggiúp trả lời "Cách xây dựng thành phần phần mềm?"

Wikipedia định nghĩa là
> Trong lĩnh vực kĩ thuật phần mềm, structural design pattern là các design pattern được thiết kế dễ dàng bằng cách xác định đơn giản các mối quan hệ giữa các thực thể. 

 * [Adapter](#-adapter)
 * [Bridge](#-bridge)
 * [Composite](#-composite)
 * [Decorator](#-decorator)
 * [Facade](#-facade)
 * [Flyweight](#-flyweight)
 * [Proxy](#-proxy)

🔌 Adapter
-------
Ví dụ thực tế
> Giả sử là bạn đang có một số hình ảnh trong thẻ nhớ của mình và bạn cần chuyển chúng vào máy tính. Để chuyển được chúng bạn cần có thứ gì đó như adapter có khả năng tương thích với máy tính của mình để bạn có thể kết nối thẻ nhớ vào máy tính. Trong trường hợp này đầu đọc thẻ là một adapter.

> Một ví dụ khác như bộ nguồn adapter nổi tiếng; chiếc ổ cắm 3 chân không thể kết nối với đầu ra hai chân, nó cần sử dụng một power adapter giúp nó tương thích với đầu ra 2 chân.

> Một ví dụ khác là một người dịch giả sẽ dịch những từ do một người nói ra cho người khác.

Nói một cách đơn giản

> Adapter pattern cho phép bạn đóng gói một object không tương thích vào một adapter và giúp nó tương thích với một class khác

Wikipedia định nghĩa là
> Trong kĩ thuật phần mềm, adapter pattern là một design pattern trong lĩnh vực phần mềm cho phép interface của một class đã tồn tại có thể sử dụng được như một interface khác. Nó thường được sử dụng để giúp các class đã tồn tại làm việc được với những class khác mà không cần chỉnh sửa source code.

**Ví dụ lập trình**

Hãy xem qua một trò chơi về người thợ săn và anh ta săn sư tử.

Đầu tiên hãy tạo một interface `Lion` mà tất cả các loại sư tử có thể implement

```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```

Và thợ săn dự đoán tất cả những thứ implement từ `Lion` để săn.

```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

Bây giờ giả sử chúng ta thêm một `WildDog` vào game để thợ săn cũng có thể săn nó. Nhưng chúng ta không thể làm việc này trực tiếp vì chó thuộc một interface khác. Để nó tương thích với thợ săn của chúng ta, chúng ta sẽ tạo một adapter để nó tương thích được

```php
// Điều này cần phải được thêm vào trò chơi
class WildDog
{
    public function bark()
    {
    }
}

// Adapter xung quanh con chó hoang dã để làm cho nó tương thích với trò chơi của chúng tôi
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
```

Và bây giờ thì `WildGod` có thể được sử dụng trong game của chúng ta thông qua việc dùng `WildDogAdapter`

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 Bridge
------
Ví dụ thực tế

> Hãy xem việc bạn có một website và các trang khác nhau và bạn có nhiệm vụ phải cho phép người dùng có thể thay đổi theme. Bạn sẽ làm gì? Tạo ra một loạt các bản copy của mỗi trang cho mỗi theme hoặc bạn chỉ tạo những theme riêng và tải phần base của chúng dựa trên phần tùy chỉnh của mỗi user? Bridge pattern cho phép bạn thực hiện cách thứ 2 như này

![có hoặc không có bridge pattern](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

Nói một cách đơn giản
> Bridge pattern là về việc thích thành phần hơn inheritence (kế thừa). Chi tiết việc implement được đẩy từ một hệ thống phân cấp tới các object khác với hệ thống phân cấp riêng biệt.

Wikipedia định nghĩa là
> Bridge pattern là một design pattern được sử dụng trong kĩ thuật phần mềm mà nó được định nghĩa là "phân tách một trừu tượng từ việc thực hiện của nó để cả hai có thể khác nhau một cách độc lập"

**Ví dụ trong lập trình**

Ví dụ như việc dịch trang web của chúng ta từ trên xuống. Ở đây chúng ta có một hệ thống cấp bậc `WebPage`

```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "Careers page in " . $this->theme->getColor();
    }
}
```
Và các theme phân cấp riêng biệt 
```php

interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return 'Dark Black';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return 'Off white';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return 'Light blue';
    }
}
```

Và cả hai hệ thống phân cấp

```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 Composite
-----------------

ví dụ thực tế
> Mọi tổ chức đều bao gồm các thành viên. Mỗi một thành viên có các tính năng giống nhau như là có lương, có một số trách nhiệm, có thể hoặc không thể báo cáo cho ai đó, có thể hoặc không thể có một vài cấp dưới...

Nói ngắn gọn
> Composite pattern cho phép client xử lý các đối tượng theo một cách thống nhất.

Wikipedia định nghĩa là
> Trong kĩ thuật phần mềm, composite pattern là một design pattern thuộc nhóm phân vùng. Composite pattern mô tả về một nhóm các object được xử lý cùng một cách giống như một instance của object. Mục đích của composite là "tạo ra" các object vào một cấu trúc dạng cây để đại diện cho toàn bộ hệ thống phân cấp. Việc triển khai composite pattern cho phép client xử lý các đối tượng và bố cục riêng lẻ một cách thống nhất.

**Ví dụ trong lập trình**

Lấy ví dụ về nhân viên ở phía trên. Ở đây chúng ta có các loại nhân viên khác nhau

```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

Sau đó chúng ta có một tổ chức với nhiều kiểu nhân viên khác nhau

```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

Và nó có thể được sử dụng như sau:

```php
// Prepare the employees
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// Add them to organization
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Net salaries: " . $organization->getNetSalaries(); // Net Salaries: 27000
```

☕ Decorator
-------------

Ví dụ thực tế
> Hãy tưởng tượng bạn đang có cửa hàng dịch vụ xe hơi và cung cấp nhiều dịch vụ khác nhau. Bây giờ bạn phải tính hóa đơn như nào? Bạn chọn một dịch vụ và tự động bổ sung giá của các dịch vụ đã cung cấp cho đến khi bạn nhận được chi phí cuối cùng. Ở đây mỗi loại dịch vụ là một decorator.

Nói ngắn gọn

> Decorator pattern cho phép bạn tự động thay đổi các hành vi của một object ngay trong khi đang chạy bằng việc đóng gói chúng vào trong một object của một class decorator. 

Wikipedia định nghĩa là

> Trong lập trình hướng đối tượng, decorator pattern là một design pattern mà cho phép hành động thêm vào các object riêng lẻ, tĩnh hoặc động mà không ảnh hưởng lên hành vi của các object khác trong cùng class. Decorator pattern khá hữu dụng trong việc tôn trọng nguyên tắc Single Responsibility Principle, vì nó cho phép các chức năng được phân chia giữa các class mà nó quan tâm tới những khu vực duy nhất

**Ví dụ trong lập trình**

Lấy coffee  là ví dụ. Đầu tiên tất cả chúng ta có một cốc coffee đơn giản được implement từ interface coffee .

```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return 'Simple coffee';
    }
}
```

Chúng ta muốn có thể mở rộng code để cho phép sửa đổi các tuỳ chọn nếu nó được yêu cầu.  Hãy tạo ra một vài add-on (decorator).

```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```

Giờ hãy tạo ra một ly coffee nào

```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```
📦 Facade
----------------

Ví dụ thực tế
>Làm thế nào để bạn bật máy tính? "Nhấn nút nguồn" bạn sẽ nói! Đó là điều bạn tin bởi vì bạn đang sử dụng một giao diện đơn giản mà máy tính cung cấp ở bên ngoài, bên trong nó phải làm rất nhiều thứ để làm cho nó xảy ra. Giao diện đơn giản này với hệ thống con phức tạp là một facade.

Nói một cách đơn giản
> Facade pattern cung cấp giao diện đơn giản cho một hệ thống con phức tạp.

Theo Wikipedia :
> facade là một đối tượng cung cấp một interface đơn giản hóa cho đoạn code lớn, chẳng hạn như một class library.

**Ví dụ về lâp trình**

Lấy ví dụ về máy tính ở trên. Ở đây chúng ta có class computer

```php
class Computer
{
    public function getElectricShock()
    {
        echo "Ouch!";
    }

    public function makeSound()
    {
        echo "Beep beep!";
    }

    public function showLoadingScreen()
    {
        echo "Loading..";
    }

    public function bam()
    {
        echo "Ready to be used!";
    }

    public function closeEverything()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth()
    {
        echo "Zzzzz";
    }

    public function pullCurrent()
    {
        echo "Haaah!";
    }
}
```
Ở đây chúng ta có facade
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```
Bây giờ sử dụng facade như sau :
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

🍃 Flyweight
---------

Ví dụ thực tế
> Bạn đã từng uống trà tươi từ một số gian hàng chưa? Họ thường làm nhiều hơn một ly mà bạn yêu cầu và để phần còn lại cho bất kỳ khách hàng nào khác để tiết kiệm tài nguyên, ví dụ: gas, vv .Flyweight pattern là tất cả về điều đó tức là chia sẻ.

Nói một cách đơn giản
> Nó được sử dụng để giảm thiểu sử dụng bộ nhớ hoặc chi phí tính toán bằng cách chia sẻ càng nhiều càng tốt với các đối tượng tương tự.

Theo Wikipedia : 
> Trong lập trình máy tính, flyweight là một mẫu thiết kế phần mềm.. Flyweight là một đối tượng giảm thiểu việc sử dụng bộ nhớ bằng cách chia sẻ càng nhiều dữ liệu càng tốt với các đối tượng tương tự khác; nó là một cách để sử dụng các đối tượng với số lượng lớn khi mà nó lặp lại thì sẽ sử dụng một lượng bộ nhớ không thể chấp nhận được.

**Ví dụ về lập trình **

Từ ví dụ về trà ở trên. Trước hết, chúng ta có các loại trà và máy pha trà

```php
// Bất cứ điều gì sẽ được lưu trữ là flyweight.
// Các loại trà ở đây sẽ là flyweights.
class KarakTea
{
}

//Hành vi như một nhà máy và tiết kiệm trà
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

Sau đó, chúng tôi có `TeaShop` nhận đơn đặt hàng và phục vụ .

```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "Serving tea to table# " . $table;
        }
    }
}
```
Và nó có thể được sử dụng như dưới đây :

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 Proxy
-------------------
Ví dụ thực tế :
> Bạn đã bao giờ sử dụng một thẻ truy cập để đi qua một cánh cửa? Có nhiều tùy chọn để mở cánh cửa đó, tức là nó có thể được mở bằng cách sử dụng thẻ truy cập hoặc bằng cách nhấn một nút để vượt qua bảo mật. Chức năng chính của cửa là để mở nhưng có một proxy được thêm vào đầu nó để thêm một số chức năng. Hãy để tôi giải thích rõ hơn bằng cách sử dụng ví dụ code bên dưới

Nói một cách đơn giản
> Sử dụng proxy pattern, một class đại diện cho chức năng của một class khác.

Theo Wikipedia:
> Một proxy, ở dạng tổng quát nhất của nó,là một class có chức năng như một interface cho một cái khác. Proxy là một đối tượng bao bọc hoặc tác nhân được gọi bởi máy khách để truy cập đối tượng phục vụ thực đằng sau hậu trường.Sử dụng proxy chỉ đơn giản là có thể chuyển tiếp đến đối tượng thực, hoặc có thể cung cấp thêm logic. Trong chức năng bổ sung proxy có thể được cung cấp, ví dụ như bộ nhớ đệm khi các hoạt động trên đối tượng thực là tài nguyên chuyên sâu, hoặc kiểm tra điều kiện tiên quyết trước khi hoạt động trên đối tượng thực được gọi

**Ví dụ về lập trình**

Lấy ví dụ cửa an ninh của chúng ta ở trên. Đầu tiên chúng ta có interface cửa và những thứ implement nó

```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "Opening lab door";
    }

    public function close()
    {
        echo "Closing the lab door";
    }
}
```
Sau đó, chúng ta có một proxy để bảo đảm bất kỳ cửa nào mà chúng ta muốn
```php
class SecuredDoor
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "Big no! It ain't possible.";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```
Và đây là cách nó có thể được sử dụng :
```php
$door = new SecuredDoor(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```
Tuy nhiên, một ví dụ khác sẽ là một số loại triển khai trình ánh xạ dữ liệu. Ví dụ, gần đây tôi đã thực hiện một ODM (Object Data Mapper) cho MongoDB bằng cách sử dụng mẫu này, nơi tôi đã viết một proxy xung quanh các lớp mongo trong khi sử dụng phương thức ma thuật __call (). Tất cả các lời gọi phương thức đã được ủy nhiệm cho lớp mongo ban đầu và kết quả được truy xuất được trả về vì nó là nhưng trong trường hợp  `find` hoặc dữ liệu findOne được ánh xạ tới các đối tượng lớp được yêu cầu và đối tượng được trả về thay cho Cursor.


## License

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
