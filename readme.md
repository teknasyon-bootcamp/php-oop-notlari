# PHP İLE NESNE TABANLI PROGRAMLAMA NOTLARI

## Nesne tabanlı/yönelimli programlama nedir? Neden ihtiyaç duyarız? 

Nesne yönemli programlama genelde kısa adı olan OOP (İng. Object Oriented Programming) ile anılmaktadır. OOP için biz ölümlülerin karmaşık projelerdeki sıkıntılı olayları gerçek hayata yakınsayarak çözme çabası desek herhalde sürçülisan etmeyiz.

Bu olay biz geliştiricilerin hayatını o kadar kolaylaştırıyor ki anlatırken kendimi Eminönü'nde turistlere mal satmaya çalışan esnaf gibi hissediyorum. - [@haydar](https://github.com/haydar)

Bu yaklaşımı anlatırken hayvanı korkutmamak adına tümevarım tekniğini kullanacağım. :sweat_smile: Korkmayın dinozor avlamak kadar zor değil.  :

![](https://media.giphy.com/media/ncgNxgevNmfJ8e6VWl/giphy.gif?cid=790b7611b02e8e28a71e1e766219aafac5e4e4e9c09f43e3&rid=giphy.gif&ct=g)

Mevzuya girmeden önce bilememiz gereken ilk şey sınıf ve nesnelerin ne olduğudur.



## Sınıf (class) nedir?

Sınıflarımızı, programımıza anlatmak istediğimiz şeylerin somut taslakları olarak düşünebiliriz. Sınıflar betimledikleri şeylerle ilgili hangi verileri ve işlevleri içericeklerini belirtirler.  Sınıf adları kural olmasa da büyük harfle başlar.

Örneğin aşağıdaki kod parçasını ele alalım. 

```php 

class Animal{

    public int $age;
    public string $species;
    public const LIVE_IN = "Dünya";

    public function born() {
        echo "Hayvan doğdu. <br/>";
    }

    public function die() {
        echo "Hayvan öldü. <br/>";
    }    
}
```

Burada Hayvan sınıfımızı **class** anahtar kelimesiyle oluşturduk. Bu sınıfta olmasını istediğimiz özellikleri/nitelikleri yani **property**'lerimizi belirttik. Bu nitelikle yukarıda gördüğünüz gibi değişkenler veya sabitlerden olabileceği gibi başka bir sınıfın nesneleri de olabilir.  Bir de bunların başında **public** diye bir şey yazıyor, ne olduğuna sonra değineceğiz.

## Nesne (Object) nedir?

Nesneler taslaklarımız olan sınıflarımızın örneklemidir. (İng. Instance) Daha basit bir şekilde anlatmak gerekirse oturduğunuz evin projesi sınıfsa, oturduğunuz ev sınıfın nesnesidir. Bu arada nesne ve örneklem aynı şeyi ifade ediyor. Yani instance eşittir object diyebiliriz.

Öncelikle **new** anahtar kelimesiyle (İng. Keyword) sınıfımızdan nesnelerimizi oluşturabiliriz. Nesnelerimizin property'lerine ve metotlarına **->** ve sabitlerine ise tüm nesnelerde aynı kalacağından **::** karakterleriyle erişebiliriz. 


Yukarıdaki sınıf örneğini devam ettirerek gidelim. Hayvan sınıfımızdan kuş ve köpek adında iki nesne oluşturalım. Bunlar tüm hayvan özelliklerine sahip olduklarından hayvan sınıfı içerisindeki tüm metotlara (Bir sınıfın fonksiyonlarına metot denir.) erişebilir. 


```php 

class Animal{

    public int $age;
    public string $species;
    public const LIVE_IN = "Dünya";

    public function born() {
        echo "Hayvan doğdu. <br/>";
    }

    public function die() {
        echo "Hayvan öldü. <br/>";
    }    
}

$bird = new Animal();
$bird->born();
$bird->species = "Kuş";
$bird->age =  1;
$bird->die();
echo  "Kuşlar ". $bird::LIVE_IN . "'da yaşarlar <br/>";

$dog = new Animal();
$dog->born();
$dog->species = "Köpek";
$dog->age = 3;
$dog->die();
echo  "Köpekler ". $bird::LIVE_IN . "'da yaşarlar <br/>";

```
Yukarıdaki kodun çıktısı şu şekilde olacaktır. 
```
Hayvan doğdu.
Hayvan öldü.
Kuşlar Dünya'de yaşarlar
Hayvan doğdu.
Hayvan öldü.
Köpekler Dünya'de yaşarlar
```

Peki ben nesnelerimin sonunda yazdırdığım mesajımı "Ben bir <nesnenin nerede yaşadığı>yım ve Dünya'da yaşarım" olarak değiştirip, bunu hayvan sınıfımın içerisinde tanımladığım bir fonksiyonla yaptırabilir miyim? Hem böyle her nesne için aynı şeyi tekrar yazmamış olacağım. Bunun için **$this** anahtar sözcüğünün ne işe yaradığını bilmemiz gerekiyor. $this içinde bulunduğu sınıftan türetilen nesneyi ifade eder. Bu tanımdan bir şey anlamdıysanız telaşa gerek yok. Sakin olun ve aşağıdaki örneği inceleyin.

```php 
class Animal{

    public int $age;
    public string $species;
    public const LIVE_IN = "Dünya";

    public function born() {
        echo "$this->species doğdu. <br/>";
    }

    public function die() {
        echo "$this->species öldü. <br/>";
    }  

    public function greet(){
        echo "Ben  ". $this->species." türündenim ve ". 
        $this::LIVE_IN ."'da yaşıyorum <br/>";
    }
}

$bird = new Animal();
$bird->species = "Kuş";
$bird->born();
$bird->age =  1;
$bird->greet();
$bird->die();

$dog = new Animal();
$dog->species = "Köpek";
$dog->born();
$dog->age = 3;
$dog->greet();
$dog->die();
```
Çıktımız şu şekilde olacaktır. 
```
Kuş doğdu.
Ben Kuş türündenim ve Dünya'da yaşıyorum
Kuş öldü.
Köpek doğdu.
Ben Köpek türündenim ve Dünya'da yaşıyorum
Köpek öldü.
```

Bir şeyler canlandı mı? Peki o zaman, güzel... Nesne ile sınıfın ne olduğunu anladığımıza göre **static** şey de nedirmiş öğrenelim...

## Statik (static) olmak nedir?

Herhangi bir nesne oluşturmadan kullanılabilmeye yapılara static (Tr. Durağan) deriz. Yani static property ve metotlar nesnenin oluşup oluşmadığına bakmazlar. Static yapılarda herhangi bir nesne oluşturma şartı olmadığı için sınıf içinde çağrılar $this anahtar kelimesi yerine self anahtar kelimesiyle veya sınfın adıyla, sınıf dışında ise yalnızca sınıfın adıyla gerçekleşir. 

Sınıfın içinde ;

```php 
self::const //Statik sabitlere erişim.
self::$variable // Statik değişenlere erişim
self::method() // Statik metotlara erişim
ClassName::const //Statik sabitlere erişim.
ClassName::$variable // Statik değişenlere erişim
ClassName::method() // Statik metotlara erişim
```

Sınıfın dışında ;

```php 
ClassName::const //Statik sabitlere erişim.
ClassName::$variable // Statik değişenlere erişim
ClassName::method() // Statik metotlara erişim
```


Çağrı şekillerini de gördüğümüze göre hemen bir örnekle açıklayalım.

```php 
class Animal{

    public int $age;
    public string $species;
    public const LIVE_IN = "Dünya";
    public static int $counter = 0;

    public function born() {
        echo "$this->species doğdu. <br/>";
    }

    public function die() {
        echo "$this->species öldü. <br/>";
    }  

    public function greet(){
        echo "Ben  ". $this->species." türündenim ve ".
        $this::LIVE_IN ."'da yaşıyorum <br/>";
    }

    public static function increaseCounter()
    {
        echo self::class." sınıfının sayacı arttırılıyor. <br/>";

        self::$counter++;
        // Does samething with above code
        Animal::$counter++;

        echo "Sayacın şu an ki değeri : " .self::$counter."<br/>";
    }
}

Animal::increaseCounter();
Animal::increaseCounter();
Animal::$counter += 20;
Animal::increaseCounter();
```

Yukarıdaki önce kod sayacı arttır metodunu kullanarak sayaç statik değişkenini ikişer ikişer arttırıp dörde eşitliyor. Daha sonra sınıf dışından bu değişkene 20 değerini ekliyor. En sonra bir kez daha ikişer arttırıyor.

Çıktısı da şu şekilde; 

```
Animal sınıfının sayacı arttırılıyor.
Sayacın şu an ki değeri : 2
Animal sınıfının sayacı arttırılıyor.
Sayacın şu an ki değeri : 4
Animal sınıfının sayacı arttırılıyor.
Sayacın şu an ki değeri : 26
```

## Kalıtım / Miras alma (Inheritance) nedir? 

Ortak özelliklere sahip yapıların birbirinden türetilmesine denir. Örneğin; Otobüs ve otomobil birçok ortak özellikleğe ve işleve sahip olduğu için aynı şeyleri ikisine ayrı ayrı tanımlamak yerine Araç adında bir sınıfta bunları tutabiliriz. Daha sonra bu Araç sınıfından kalıtım/miras alarak özellikler ve işlevleri Otobüs ve Otomobil sınıflarına aktarabiliriz. 

Kalıtım alan sınıfa çocuk sınıf (İng. **Child class**), katılım alınan sınıfa ise ebeveyn sınıf (İng. **Parent class**) denir. Ebeveyn sınıftaki üyelere **parent::** anahtar kelimesiyle erişilir.

Sınıf içinde bir sınıf öyesini çağırıyorsak **self::** veya **static::** anahtar kelimelerini kullanabiliriz. static çağrıldığı sınıfı döndürürken, self yazılı olduğu sınıfı döndürür. Bu örnek daha açıklayıcı olacaktır. 

```php 
class A
{
    public static function getStaticName()
    {
        return static::class . " sınıfından çağrıldım <br/>";
    }

    public static function getName()
    {
        return self::class . " sınıfında çalıştım <br/>";
    }
}

class B extends A
{
}

echo B::getStaticName();
echo B::getName();

```
Ekran çıktsı şu şekildedir;

```
B sınıfından çağrıldım
A sınıfında çalıştım
```
Sadece bir sınıftan kalıtım alınabilir ancak örnek verecek olursak Çocuk sınıfı baba sınıfı, baba sınıfını kalıtım alabilir. Baba sınıfı da dede sınıfını kalıtım alabilir. Yani kalıtım sırası şu olabilir; 



`Çocuk->Baba->Dede`

Buna rağmen çocuk sınıf hem babayı hem de dedeyi kalıtım alamaz. ([@kadirermantr](https://github.com/kadirermantr) 'ye selamlar :sunglasses:) Yani şu olamaz.

```
        |====>Baba
Çocuk =>| 
        |====>Dede  
```

Katılım **extends** anahtar kelimesiyle alınır. Yine bir örnekle pekiştirelim.

```php
class Animal{

    public int $age;
    public string $species;
    public const LIVE_IN = "Dünya";

    public function born() {
        echo "$this->species doğdu. <br/>";
    }

    public function die() {
        echo "$this->species öldü. <br/>";
    }  

    public function greet(){
        echo "Ben  ". $this->species." türündenim ve ". 
        $this::LIVE_IN ."'da yaşıyorum <br/>";
    }
}

class Bird extends Animal{

}

$pigeon = new Bird();
$pigeon->species = "Güvercin";
$pigeon->born();
$pigeon->age =  1;
$pigeon->greet();
$pigeon->die();
```

Çıktımız şu olacaktır.

```
Güvercin doğdu.
Ben Güvercin türündenim ve Dünya'da yaşıyorum
Güvercin öldü.
```

Yukarıdaki örneğe baktığımızda kuş sınıfıyla oluşturduğumuz nesneden çağırdığımız hiç bir sabit (İng. constant), özellik (İng. Property) ve metodun (İng. Method) kuş sınıfı içerisinde yazılmadığını görürüz. Fakat çalışıyor çünkü tüm bu özellik ve işlevleri hayvan sınıfından kalıtım alındı. 

Fakat burada aklımıza şöyle bir soru gelebilir. Güvercin (İng. Pigeon) nesnesini doğrudan hayvan sınıfıyla da oluşturabilirdim. Peki neden ayrıyetten kuş diye bir sınıf oluşturdum? Bu örnekte haklı olsanız da bir sonraki başlık bu sorunuzun cevabını verecek...

## Çok Biçimlilik (Polymorphism) nedir?

Sınıfım biçim biçim ölürüm nesnem için, kalıtım bana düşmandır, sevdiğim nesnem için... 

Goy goyu bir kenara bırakırsak kalıtım alan bir sınıf, kalıtım özellikleri ve işlevleri değiştirmek veya yeni özellikler vermek isteyebilir.

Haydi bunu yapalım bakalım. 

```php

class Animal{

    public int $age;
    public string $species;
    public const LIVE_IN = "Dünya";

    public function born() {
        echo "$this->species doğdu. <br/>";
    }

    public function die() {
        echo "$this->species öldü. <br/>";
    }  

    public function greet(){
        echo "Ben  ". $this->species." türündenim ve ".
        $this::LIVE_IN ."'da yaşıyorum <br/>";
    }
}

class Bird extends Animal{

    //Override species variable
    public string $species = "Değiştirilen Kuş";

    public function tweet()
    {
        echo "Cik cik";
    }

    // Override greet method
    public function greet(){
        echo "Selamlaşma metodunu ezdim. <br/>";
    }

}

$pigeon = new Bird();
$pigeon->born();
$pigeon->greet();
$pigeon->tweet();
```

Ahanda çıktısı şu şekilde olacaktır.

```
Değiştirilen Kuş doğdu.
Selamlaşma metodunu ezdim.
Cik cik
```

Örnekte kuş sınıfına cıvıldama (İng. Tweet) fonksiyonunu eklediğim için güvencin adlı nesneden bu fonksiyonu çağırabildim. Hayvan sınıfından gelen tür özelliğini ezerek (İng. **Override**) varsayılan bir değer verdim. Böylelikle kullanıcıdan değer almama gerek kalmadı. Üstüne üstük selamlaşma metodunu yeniden yazarak, hayvan sınıfından kalıtım aldığım selamlamlaşma metodunu ezdim. 

Bu konuda dikkat etmemiz gerekn iki şey var. 

- Bir niteliği ezerken farklı bir veri tipi veremezsiniz. Yani ebeveyn sınıfta string olarak belirlilen bir özelliği çocuk sınıfta integer değer yapamazsınız. 
- Bir metodu ezerken metodun kapsamını genişletebilir fakat küçültemezsiniz. Yani ebeveyn sınıfta 2 tane parametre alan bir metodu çocuk sınıfta 3 tane parametre alan bir metoda çevirebilir ama 1 tane parametre alan metoda çeviremezsiniz. Ebeveyn sınıftaki fonksiyona gelen parametrelerin adlarını değiştirebilir fakat değer tiplerini değiştiremezsiniz.

**Not :** Ezme kurallarında ileride göreceğimiz __construct metodu istisnadır.


## Kapsülleme (Encapsulation) nedir? 

Kapsülleme, ha! Vay anam, babam ne kadar da havalı bir isim! Olayın ismi sizi aldatmasın yaptığı işin tanımı çok basit. Sınıflardaki özellik (property) ve işlevlerin (method) başka yerlerden erişilip erişilemeyeceğini denetleyen yapıdır. Yani her önüne gelen istediğine erişemeyecek.
Olayı kurgulamak için örnekte kullanacağımız bir metot ve bir konu sıkıştıracağım. 

### Erişim belirliyiciler (Access Modifiers) 

Başlarına geldiği yapının nerelerden erişilebileceğini belirleyen anahtar kelimelerdir.

**public :** Varsayılandır. Eğer erişim belirliyicisi olarak bir şey yazmadıysanız programlama dili public atar. Her yerden erişilebilen özellik ve davranışları belirtir.

**protected :** Tanımladığı sınıfı miras alan diğer alt sınıflar tarafından kullanılabileceğini belirtir.

**private :** Etliye sütlüye karışmadığını sadece tanımlandığı sınıfta erişilebildiğini belirtir. 

### Kurucu metot (__construct) nedir?

**new** anahtar kelimesiyle bir metot oluşturulduğunda otomatik çalıştıralacak metottur. Kurucu metotta istediğimiz parametreleri nesneyi oluşturken () arasına ekleriz. Dikkat ederseniz static fonksiyonları çağrılırken bir nesne oluşturmadığınızdan kurucu metot çağrılmayacaktır.

```php 
class Animal{

    public function __construct()
    {
        echo self::class." doğdu. <br/>";
    }
}
$myAnimal = new Animal();
```
Yukarıdaki kodun çıktısı şu şekilde olacaktır. 

`Animal doğdu.`

Şimdi kapsüllemeye geri dönelim. Ne demiştik? Ha, önüne gelenin erişmesini engelleyeceğiz demiştik. Şimdi örneğimize bakalım. 

```php 
class Animal{

    private int $age;
    private string $species;

    public function __construct(int $age, string $species)
    {
        $this->age = $age;
        $this->species = $species;
    }

    public function getAge() : int
    {
        return $this->age;
    }

    public function setAge($age) : void
    {
        $this->age = $age;
    }

    public function getSpecies() : string
    {
        return $this->$species;
    }

    public function setSpecies($species) : void
    {
        $this->species = $species;
    }

    public function whoami() : void
    {
        $this->privateWhoami();
    }

    private function privateWhoami() : void
    {
        echo "Ben $this->species türündenim ve $this->age yaşındayım <br/>";
    }
}

$myAnimal = new Animal(1,"Köpek");
$myAnimal->whoami();


/**
 * $myAnimal->age = 2; Uncaught Error: Cannot
 * access private property 
 * 
 * $myAnimal->species = "At"; Uncaught Error: Cannot 
 * access private property 
 */

$myAnimal->setAge(2);
$myAnimal->setSpecies("At");

/**
 * echo "Yeni Tür : $myAnimal->species"; Uncaught Error: Cannot 
 * access private property 
 * 
 * echo "Yeni Yaş : $myAnimal->age"; Uncaught Error: Cannot 
 * access private property 
 */

echo "Yeni Tür : ". $myAnimal->getSpecies()."<br/>";
echo "Yeni Yaş : ". $myAnimal->getAge()."<br/>";

// $myAnimal->privateWhoami(); Uncaught Error: Call to private method 
$myAnimal->whoami();
```

Kodumuzun ekran çıktısı şu şekildedir; 

```
Ben Köpek türündenim ve 1 yaşındayım
Yeni Tür : At
Yeni Yaş : 2
Ben At türündenim ve 2 yaşındayım
```

Dikkat ettiyseniz önceki örneklerden farklı olarak propertlerimi **public** yerine **private** tanımladım. Yani bu propertylerime sınıf dışından erişimi kapattım. Ayrıca **private** olarak *privateWhoami* adında bir metot tanımladım. Koda baktığımızda private olan sınıf üyelerine (property ve metotlar) herhangi bir şekilde erişmeye çalıştığımda hata aldığımızı görüyorsunuz. 

Peki nasıl bunlara eriştik? Private erişim belirliyici ile erişimi sınıf dışına engelledik, sınıf içinden hala erişebiliyoruz. Bu sınıf üyelerine dışarıdan çağırabildiğimiz metotlar yardımıyla erişebiliriz. Bu şekilde properylerde  değer atama ve okuma yazma yapabiliriz. Bu metotlara **getter** ve **setter** yani alıcı ve atayıcı metotlar denir. Aynı şekilde private bir metodu araya public bir metot koyarak dışarıdan çağırabiliriz. 


## Soyutlama (Abstraction) nedir?  

Projelerimizde çoğu zaman teke başına geliştirme yapmıyoruz. Her geliştiricinin aynı yapıya uygun şekilde kod yazması için yapıyı açıkladığımız planlara ihtiyacımız vardır. Bu nedenle yapının anlatıldığı soyut iskeletlere ihtiyaç duyarız. Bu yapıların kurulmasına soyutlama denir. 


### Soyut sınıf (Abstract Class) nedir?

Daha önce sınıflarımızın nasıl çalışıtığını görmüştük. Abstract sınıflar ve fonksiyonlar normal fonksiyon ve sınıflardan farklı olarak başlarına **abstract** anahtar kelimesi getirerek oluşturulur. Soyut bir yapıda olabilmesi için en az bir adet soyut metot içermesi gerekmektedir. Soyut metodun işlevi soyut sınıfta tanımlanmaz. Sadece bu adda varsa şu parametleri alan bir metoddur diye tanımlanabilir. Soyut metodun dışında somut metotlar da içerebilir. Soyut bir sınıftan **new** anahtar sözcüğü ile yeni bir nesne **oluşturalamaz.**

Haydi örneğimiz bir göz atalım. 


```php 

abstract class Animal{
    protected string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    abstract public function talk();

    public function greet()
    {
        echo "Selamlar... <br/>";
    }
}

class Cat extends Animal
{
    public function talk()
    {
        echo $this->name. ":  Miyav miyav... <br/>";
    }
}

class Dog extends Animal
{
    public function talk()
    {
        echo $this->name. ":  Hav hav... <br/>";
    }
}

// $myAnimal = new Animal(); Cannot instantiate abstract class 

$myCat = new Cat('Sevimli kedi maviş');
$myCat->talk(); // Abstract method
$myCat->greet(); // Concrete class
$dog = new Dog('Akıllı köpüş şanslı');
$dog->talk(); // Abstract method
$dog->greet(); // Concrete class
```

Kodun çıktısı şu olacaktır. 

```
Sevimli kedi maviş: Miyav miyav...
Selamlar...
Akıllı köpüş şanslı: Hav hav...
Selamlar...
```

Önce örnekteki soyut sınıfın içine odaklanalım. construct ve greet olmak üzere iki somu ve talk adında bir soyut fonksiyonum var. Talk metodu soyut olduğundan adını vermek dışında bir tanımlama yapmadık. Bunun yerine yapacağı işlevleri soyut sınıfımızı kalıtım alan çocuk sınıflarda yaptık. Zaten yapmadığımız takdirde kod hata verecekti. Somut fonksiyonlar için ise böyle bir şey yapmama gerek yok.



Soyut sınıflar başka bir soyut sınıfta veya  birden fazla **arayüzden** kalıtım alabilirler fakat aynı isimde metot içeremezler. 

### Arayüz (interface) nedir? 


Arayüzler kendinden kalıtım alan sınıfların hangi işlevleri yerine getireceğini gösteren bir katologdur. **interface** anahtar kelimesiyle oluşturulurlar. Arayüzde yazılan işlevlerin içeriği yazılmaz, yalnızca imzaları yazılır. İşlevleri kalıtım alan sınıflar işlevlerin içeriğini kendileri belirtmek zorundadırlar. Arayüzler new anahtar kelimesiyle nesne türetemezler. Arayüz içindeki tüm metotların erişim belirleyicisi public olmak zorundadır. Arayüz içinde özellikler bulunmaz ancak bir istisna olarak sabit (const) tanımı yapılabilir. Arayüzler extends ile başka bir arayüz tarafından kalıtım alınabilir.  PHP’de arayüzler diğer OOP destekleyen dillerden farklı olarak static metotlara sahip olabilir. Arayüzler sınıflar tarafından **implements** anahtar kelimesi ile kalıtım alınabilirler.
 
 ```php
 interface Person {
 
	public function __construct($name);
	public function greet() : string;
}

class Programmer implements Person {

	public $name;
	public function __construct($name) {
		$this -> name = $name;
	}
	public function greet() : string {
		return "Hello World from " . $this -> name;
	}
}

$programmer = new Programmer('Betül');
echo $programmer -> greet();
```

Bir sınafın birden fazla arayüzden kalıtım alması durumunu inceleyelim.

```php
Interface MyInterface1 {

	public function myMethod1();
}
Interface MyInterface2 {

	public function myMethod2();
}
class MyClass implements MyInterface1, MyInterface2 {

	public function myMethod1() {
		echo "Hello ";
	}
	public function myMethod2() {
		echo "World";
	}
}

$obj = new MyClass();
$obj -> myMethod1();
$obj -> myMethod2();
```

Çıktımız şu olacaktır;

`Hello World`

#### Sınıftan ve arayüzden birlikte miras almak
```php
Interface MyInterface {

	public function write();
}
class ParentClass {

	public $name;
	public function __construct($name) {
		$this -> name = $name;
	}
}
class ChildClass extends ParentClass  implements MyInterface {

	function write() {
		echo $this -> name;
	}
}

$child = new ChildClass('Hyvor');
$child->write();
```

Çıktısı şu şekilde olacaktır;

`Hyvor
`
### Arayüz ve soyut sınıfın farkları


| INTERFACE CLASS                                  | ABSTRACT CLASS              | 
| -------------------------------------------------| ----------------------------|
| Interface  çoklu kalıtım destekler| Abstract sınıf, birden çok mirası desteklemez |                    
|Sabitler dışında veri üyesi (property) içermez.|Abstract sınıf bir veri üyesi içerir|
|Bir interface sınıfı, yalnızca üyenin imzasına atıfta bulunan tamamlanmamış üyeler içerir.|Abstract sınıf hem eksik (yani soyut) hem de eksiksiz üyeler içerir.|
|Her şeyin genel olduğu varsayıldığından, bir interface sınıfının varsayılan olarak erişim değiştiricileri yoktur.|Absract bir sınıf, alt öğeler, fonksiyonlar ve özellikler içinde erişim değiştiricileri içerebilir.

#### SONUÇ OLARAK
Temel bir şeyi unutmamalıyız: bunlar birbirinin alternatifi olarak kullanılamayacak tamamen farklı iki yapıdır.
 
Arayüz sınıfları, alt sınıfların kendileri için her şeyi uygulamasını bekleyen içi boş kabuklardır. Soyut sınıflar, yalnızca kabuklar arasındaki ortak bilgi parçasını içermekle kalmaz, aynı zamanda içindeki alt sınıfların boşlukları doldurmasını bekler.
### TRAITS

![](https://i.imgur.com/ZXpZARB.png)

Yukarıdaki görüntü, Sınıf B ve Sınıf C'nin A Sınıfından nasıl miras alındığını ve Sınıf D'nin Sınıf B ve Sınıf C'den miras almak istediğini gösterir. Ancak, A işlevini çalıştırmak istediğimizde A() işlevinin hangi sürümünü çağıracağız( )? B'den mi yoksa C'den mi? Bu aslında Çoklu kalıtım problemidir ve PHP de çoklu kalıtımı desteklemez ve neden desteklemediğini görüyoruz, ancak kodun yeniden kullanılabilirliği için birden fazla sınıfa genişletmek isteyeceğiniz ve DRY (Kendinizi tekrar etmeyin) konseptini takip etmek isteyeceğiniz birçok durum var.

![](https://i.imgur.com/POqJlhH.png)

PHP'de bir sınıf sadece diğer bir sınıftan miras alabilir. Yukarıdaki görüntü, Sınıf D'nin Sınıf C'den nasıl miras aldığını gösterir, bu nedenle A() fonksiyonu, C Sınıfının A fonksiyonundan çağrılır. Bir veya daha fazla sınıftan miras almak istersek ne olur? Tekli kalıtım sorunu budur. Peki bunu nasıl çözeceğiz, işte kahramanımız Traitler oluyor.Traits bir sınıfa çok daha benzer, ancak yalnızca metodların ayrıntılı ve tutarlı bir şekilde gruplandırılması içindir Bir sınıf gibi bir trait'i somutlaştıramazsınız, aslında, bir traiti kendi başına somutlaştırmak mümkün değildir. PHP'de özellikleri nasıl uygulayabileceğimize hızlıca bir göz atalım.

```php
trait testtrait{
    public function test1(){
    echo "test1 method in trait\n";
   }
    public function test2(){
    echo "test2 method in trait\n";
  }
}
//using trait

class testclass{

   use testtrait;
   public function test1(){
      echo "test1 method overridden\n";
   
   }
}

$obj=new testclass();
$obj->test1();
$obj->test2();
```
Çıktımız ;

```
test1 method overridden
test2 method in trait`
```

Bu kez de traitten gelen metotları ezmeye çalışalım.

```php
//definition of trait

trait testtrait{

   public function test1(){
      echo "test1 method in trait\n";
   }
}
//parent class

class parentclass{

   public function test2(){
      echo "test2 method in parent\n";
   }
}

//using trait and parent class

class childclass extends parentclass{

   use testtrait;
   public function test1(){
   echo "parent method overridden\n";
   }
   public function test2(){
   
      echo "trait method overridden\n";
   }
}
$obj=new childclass();
$obj->test1();
$obj->test2();
```
Çıktımız;

```
parent method overridden
trait method overridden
```

Traitlerin interface ile kullanımına bir bakalım. 

```php
//definition of trait
trait mytrait{
   public function test1(){
      echo "test1 method in trait1\n";
   }
}
class myclass{
   public function test2(){
      echo "test2 method in parent class\n";
   }
}
interface myinterface{
   public function test3();
}
//using trait and parent class
class testclass extends myclass implements myinterface{
   use mytrait;
   public function test3(){
      echo "implementation of test3 method\n";
   }
}
$obj=new testclass();
$obj->test1();
$obj->test2();
$obj->test3();
```
Çıktımız;

```
test1 method in trait1
test2 method in parent class
implementation of test3 method
```

Traitlerin metot isimleri çakırsa ne olacak? Sorun yok, çözüm var...
```php
trait trait1{
   public function test1(){
      echo "test1 method in trait1\n";
   }
   public function test2(){
      echo "test2 method in trait1\n";
   }
}
trait trait2{
   public function test1(){
      echo "test1 method in trait2\n";
   }
   public function test2(){
      echo "test2 method in trait2\n";
   }
}
//using trait and parent class
class testclass {
   use trait1, trait2{
      trait1::test1 insteadof trait2;
      trait2::test2 insteadof trait1;
   }
}
$obj=new testclass();
$obj->test1();
$obj->test2();
```

Çıktımız;

```
test1 method in trait1
test2 method in trait2
```


Trait içindeki Erişim Belirleyiciler as anahtar kelimesiyle değiştirilebilir.

```php
trait Ogrenci {

  private function AdiSoyadi() {
    return "Öğrenci Adı Soyadı";
  }

}

class UniversiteOgrencisi {

  use Ogrenci {
    Ogrenci::AdiSoyadi as public;  // private olan AdiSoyadi artık public oldu.
  }
}

$ben = new UniversiteOgrencisi();
echo $ben->AdiSoyadi();
```

Çıktımız;

```
Öğrenci Adı Soyadı
```


![](https://i.imgur.com/0KzyreP.jpg)


Şimdi dışarıya çıkın biraz yürüyün, beyninize oksijen girsin :smile:


