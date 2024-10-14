# RegardTest
########## Задача №1 ##########

Напишите функцию, которая развернёт список.
Последний элемент должен стать первым, а первый - последним. 
$c→next должен содержать $b и так далее...

<pre>
class test {
  public $next;
}

$a = new test();
$b = new test();
$c = new test();
$a->next = $b;
$b->next = $c;
$c->next = null;

function reverse($a) {
  $previous = $a;
	$temporary = $a;
	$current = $a->next;
	
	$temporary->next = null;
	
	while (!is_null($current)) {
		$temporary = $current->next;
		$current->next = $previous;
		$previous = $current;
		$current = $temporary;
	}
	
	
  return $previous;
}
$ob1 = reverse($a);
var_dump($ob1);
</pre>

########## Задача №2 ##########

Даны две модели Order и Manager
<pre>
class Order extends Model
{
    public $appends = ['manager_full_name'];
    protected $hidden = ['manager_id', 'manager'];

    public function manager()
    {
        return $this->hasOne('App\Manager', 'id', 'manager_id');
    }

    public function getManagerFullNameAttribute(): string
    {
        if (!$this->relationLoaded('manager')) {
            return '';
        } else {
            return $this->manager->full_name;
        }
    }
}

class Manager extends Model
{
    public function getFullNameAttribute(): string
    {
        return $this->first_name . ' ' . $this->last_name;
    }
}

class OrderController extends Controller
{
    public function index(): JsonResponse
    {
        return response()->json(Order::with('manager')->limit(50)->get());
    }
}
</pre>

Каждый Order имеет manager_id. Manager имеет id, firstName, lastName
Необходимо вывести 50 заказов (Order) + fullName менеджера с минимальным кол-вом запросов к бд.
Дополните класс Order.
Реализовать нужно без использование join.

########## Задача №3 ##########

Даны веса посылок $boxes и вес, который может увезти курьер $weight.
Курьер должен возить по 2 посылки, которые вместе по весу будут строго равны $weight.
Необходимо найти максимальное количество рейсов, которые курьер сможет сделать с учетом условий.

<pre>
// первые входные данные
$boxes = [1, 2, 1, 5, 1, 3, 5, 2, 5, 5];
$weight = 6;
// вторые входные данные
$boxes = [2,4,3,6,1];
$weight = 5;

public function getResult(array $boxes, int $weight): int
{
  $trips = 0;
	$boxesCount = count($boxes);
	for ($i = 0; $i < $boxesCount; $i++) {
		if ($i + 1 != $boxesCount) {
			for ($j = $i + 1; $j < $boxesCount; $j++)
			{
				if ($boxes[$i] + $boxes[$j] == $weight) {
					$trips++;
					array_splice($boxes, $j, 1);
					$boxesCount--;
					break;
				}
			}
		}
	}
	
	return $trips;
}
</pre>
