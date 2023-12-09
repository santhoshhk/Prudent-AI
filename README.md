# Prudent-AI
## Santhosh K

### Task 1

```

def find_starting_index(food, people):
    total_food = sum(food)
    total_people = sum(people)
    
    if total_food < total_people:
        return -1  
    
    n = len(food)
    
    for i in range(n):
        remaining_food = 0
        for j in range(n):
            idx = (i + j) % n
            remaining_food += food[idx] - people[idx]
            
            if remaining_food < 0:
                break 
        
        if remaining_food >= 0:
            return i 
    
    return -1  

food1 = [1, 2, 3, 4, 5]
people1 = [3, 4, 5, 1, 2]
result1 = find_starting_index(food1, people1)
print(result1) 

food2 = [1, 2]
people2 = [5, 6]
result2 = find_starting_index(food2, people2)
print(result2)

```


<img width="960" alt="image" src="https://github.com/santhoshhk/Prudent-AI/assets/114757338/ac670dae-4d6e-4755-b08b-1170143240c0">



