# Giới thiệu Thuật Toán Di Truyền <br>
Xuất phát từ ý tưởng lý thuyết tiến hóa nhà khoa học Darwin vào đầu thế kỷ 19 là sự tiến hóa của muôn loài dựa trên sự chọn lọc tự nhiên. Từ đây, các nhà khoa học máy tính đã nghĩ một thuật toán dựa trên ý tưởng này để giải quyết các bài toán tìm kiếm tối ưu trên không gian cực lớn. Tuy nhiên, giải thuật vét cạn được biết đến qua thuật toán trên đơn thuần không giải quyết được vấn đề. Qua năm tháng, một giải thuật được biết đến với tên gọi "Giải Thuật Di Truyền" cho ra giải pháp có thể được chấp nhận


# Ý Tưởng
Nhằm cho ra những cá thể có khả năng sinh sản và thích nghi với môi trường ngày càng nhiều, chúng ta thấy có các xuất hiện của những thành phần:
1.   Population - Quần thể
2.   Natural Selection - Chọn lọc tự nhiên
3.   Mutation - Đột biến
4.   Evolution - Tiến hóa

# Mối liên hệ giữa chúng
- Quần thể: Một quần thể được khởi tạo có các cá thể nhất định ban đầu và các đặc tính khác nhau. Điều này quy định sự sinh sản, sự sinh tồn, và khả năng thích ứng khác nhau.
- Chọn lọc tự nhiên: Theo thời gian, cá thể yếu hơn, không có khả năng sinh tồn sẽ bị loại bỏ bởi những tác nhân bên ngoài. Do đó, cá thể ưu việt hơn sẽ được giữ lại.
- Đột biến: Những cá thể sinh ra được thừa kế những đặc tính của thế hệ trước. Sau thời gian sinh sống, quần thể sẽ đạt mức giới hạn của các cặp gen được tạo. Lúc này, để tiến hóa, đột biến chính là một trong nhưng nguyên nhân chính, vai trò là nguyên liệu cho quá trình chọn lọc tự nhiên
- Tiến hóa: Những cá thể đột biến không phải luôn là những cá thể mạnh mẽ, chọn lọc tự nhiên sẽ chọn ra những cá thể đột biến nhưng thích nghi với môi trường mới và sống tốt hơn. Sau thời gian, những gen đột biến này sẽ chiếm ưu thế và đa số trong quần thể.


# Thuận Toán Di Truyền
Gồm 3 bước chính:
- Selection - Chọn Lọc những cá thể có fitness (hay score) tốt để lai ghép và đột biến
- Crossover - Lai Ghép: Trao đổi gen giữa hai cá thể
- Mutation - Đột Biến: đột biến gen của một cá thể
<br> Ngoài ra còn có bước population initialization (khởi tạo quần thể) nhằm tạo ngẫu nhiên quần thể ban đầu. Bước evaluation có mục đích để tính giá trị fitness cho các cá thể. Thuật Toán Di truyền không quan tâm đến cách thức tính fitness cho một cá thể, mà chỉ cần biết thông tin input và output. Ba bước chính của Thuật Toán Di truyền sẽ được lặp đi lặp lại cho đến khi nào điều kiện dừng được thỏa mãn. Điều kiện dừng có thể là số lần lặp (generation), tính bão hòa của giá trị max_fitness, hay sự hội tự của quần thể (các cá thế có fitness gần giống nhau). 

![a3.png](https://github.com/NguyenDinhTiem/Genetic_Algorithn/blob/main/a3.png)
# Cài đặt Thuật Toán

>> ## Population initialization


```python
# create population
import random

n = 6   # size of individual (chromosome)
m = 10  # size of population

def generate_random_value():
    return random.randint(0, 1)

def create_individual():
    return [generate_random_value() for _ in range(n)]

population = [create_individual() for _ in range(m)]

# print population
for individual in population:
    print(individual)
```

- Population Init: là giai đoạn khởi tạo các giá trị ngẫu nhiên với thông tin như sau - quần thể là m, cá thể là n.

## Evalution

### secret $=\sum_{i}{}(v_i)$


```python
# secret
def secret(vector):
    return sum(vector)
```

<h5><li> Evalution: hàm này dùng để tính giá trị fitness cho từng các thể.

>> ## Selection


```python
#selection
def selection(sorted_old_population):    
    index1 = random.randint(0, m-1)    
    while True:
        index2 = random.randint(0, m-1)    
        if (index2 != index1):
            break
            
    individual_s = sorted_old_population[index1]
    if index2 > index1:
        individual_s = sorted_old_population[index2]
    
    return individual_s 
```

- Selection: dùng để chọn ra các cá thể tốt nhất trong quần thể. Nguyên tắc chọn là lấy giá trị fitness cao nhất của các thể.

>> ## Crossover




```python
# crossover
def crossover(individual1, individual2, crossover_rate = 0.9):
    individual1_new = individual1.copy()
    individual2_new = individual2.copy()
    
    for i in range(n):
        if random.random() < crossover_rate:
            individual1_new[i] = individual2[i]
            individual2_new[i] = individual1[i] 
    
    return individual1_new, individual2_new
```

- Crossover: nhằm lai ghép 2 cá thể. Trong ví dụ này, binary crossover dùng để thực hiện lai tạo. Nguyên tắc hoạt động của binary crossover cho trước tỷ lệ thực hiện $R_{rc} = 0.9$, sinh ra một boolean vector $v_{cr}$ có độ dài n, trong đó mỗi phần tử chứa giá trị True hoặc False. Giá trị True cho một vị trí index nghĩa là thực hiện việc trao đổi gen ở vị trí đó giữa hai cá thể.

>> ## Mutate


```python
def mutate(individual, mutation_rate = 0.05):
    individual_m = individual.copy()
    
    for i in range(n):
        if random.random() < mutation_rate:
            individual_m[i] = generate_random_value()
        
    return individual_m
```

- Mutate: chức năng làm đột biến gen cho cá thể. Gen cần đột biến sẽ nhận một giá trị ngẫn nhiên nằm trong miền giá trị. Tương tự crossover, mutation cũng cần một boolean vector $v_{mt}$ để xác định những gen nào cần đột biến. Vector $v_{mt}$ được sinh ra một cách ngẫu nhiên theo một khả năng mutation $R_{mt}$ cho trước.

# Áp dụng thuật toán Genetic Algorithm

> ## Bài toán Dự đoán Giá Nhà


```python
# Code examples
import random

# Intititlization
n = 2                  # size of individual (chromosome)
m = 100                # size of population
n_generations = 2000   # number of generations
losses = []            # để vẽ biểu đồ quá trình tối ưu
```


```python
# Hàm load data
def load_data():
    # kết nối với file
    file = open('data.csv','r')

    # readlines giúp việc đọc file theo từng dòng , mỗi dòng là 1 chuỗi
    lines = file.readlines()
    
    areas  = []
    prices = []
    for i in range(10): 
        string = lines[i].split(',')
        areas.append(float(string[0]))
        prices.append(float(string[1]))

    # Đóng kết nối với file
    file.close()
    
    return areas, prices
```


```python
# load data
areas, prices = load_data()
```


```python
def generate_random_value(bound = 200):
    return (random.random()-0.5)*bound
```


```python
def compute_loss(individual):
    result = 65534
    
    a = individual[0]
    b = individual[1]    
    estimated_prices = [a*x + b for x in areas]
    
    # all prices should be positive numbers
    num_negetive_prices = sum(p < 0 for p in estimated_prices)
    if num_negetive_prices == 0:        
        losses = [abs(y_est-y_gt) for y_est, y_gt in zip(estimated_prices, prices)]
        result = sum(losses)
    
    return result
```


```python
def compute_fitness(individual):
    loss = compute_loss(individual)
    fitness = 1 / (loss + 1)
    return fitness
```


```python
def create_individual():
    return [generate_random_value() for _ in range(n)]
```


```python
def crossover(individual1, individual2, crossover_rate = 0.9):
    individual1_new = individual1.copy()
    individual2_new = individual2.copy()
    
    for i in range(n):
        if random.random() < crossover_rate:
            individual1_new[i] = individual2[i]
            individual2_new[i] = individual1[i]            
    
    return individual1_new, individual2_new
```


```python
def mutate(individual, mutation_rate = 0.05):
    individual_m = individual.copy()
    
    for i in range(n):
        if random.random() < mutation_rate:
            individual_m[i] = generate_random_value()
        
    return individual_m
```


```python
def selection(sorted_old_population):    
    index1 = random.randint(0, m-1)    
    while True:
        index2 = random.randint(0, m-1)    
        if (index2 != index1):
            break
            
    individual_s = sorted_old_population[index1]
    if index2 > index1:
        individual_s = sorted_old_population[index2]
    
    return individual_s 
```


```python
def create_new_population(old_population, elitism=2, gen=1):
    sorted_population = sorted(old_population, key=compute_fitness)
        
    if gen%1 == 0:
        losses.append(compute_loss(sorted_population[m-1]))
        #print("Best loss:", compute_loss(sorted_population[m-1]), sorted_population[m-1])      
    
    new_population = []
    while len(new_population) < m-elitism:
        # selection
        individual_s1 = selection(sorted_population)
        individual_s2 = selection(sorted_population) # duplication
        
        # crossover
        individual_c1, individual_c2 = crossover(individual_s1, individual_s2)
        
        # mutation
        individual_m1 = mutate(individual_c1)
        individual_m2 = mutate(individual_c2)
        
        new_population.append(individual_m1)
        new_population.append(individual_m2)            
    
    for ind in sorted_population[m-elitism:]:
        new_population.append(ind.copy())
    
    return new_population
```


```python
population = [create_individual() for _ in range(m)]
for i in range(n_generations):
    population = create_new_population(population, 2, i)
```


```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(10, 6))
ax.set_ylim((0, 14))
ax.set_xlim((0, 10))

plt.scatter(areas, prices)
plt.ylabel('Giá nhà (chục lượng vàng)')
plt.xlabel('Diện tích nhà ($x 100 m^2$)')

x_data = list(range(0, 10))
y_data = [1.24*x + 0.651 for x in x_data]
plt.plot(x_data, y_data,c='green')

plt.scatter([8], [1.24*8 + 0.651],c='red')
plt.plot([8, 8], [0, 1.24*8 + 0.651],c='orange')
plt.plot([0, 8], [1.24*8 + 0.651, 1.24*8 + 0.651],c='orange')
    
plt.show()
```

![a1.png](https://github.com/NguyenDinhTiem/Genetic_Algorithn/blob/main/a1.png)

```python
import matplotlib.pyplot as plt

plt.plot(losses[:200], c='green')
plt.xlabel('Generations')
plt.ylabel('losses')
plt.show()
```

![a2.png](https://github.com/NguyenDinhTiem/Genetic_Algorithn/blob/main/a2.png)



Bài Viết tham khảo từ tài liệu khóa học mùa thu AI Foundation Course by AI Việt Nam
