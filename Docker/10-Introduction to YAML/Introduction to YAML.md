
### YAML
- It is used to represent configuration data.
- key-value pair representation.
	```
	Fruit: Apple
	Vegetable: Carrot
	Liquid: Water
	```

![yamlproperties.png](Attachments/yamlproperties.png)

- Spacing in the configuration is critical.

![yamlkeyvalpair.png](Attachments/yamlkeyvalpair.png)

- Arrays/Lists are represented as below
	- Lists are ordered collection
```
Fruits:
-   Orange
-   Apple
-   Banana

Vegetables:
-   Carrot
-   Cauliflower
-   Tomato
```

![yamlarray.png](Attachments/yamlarray.png)

- Dictionary is represented as below
	- Stores different information/property about a single object dictionary is used
	- "Calories", "Fat" and "Carbs" are different information about "Banana".
	- "Color", "Model", "Transmission", "Price" are different properties of a car.
	- Dictionary is an unordered collection
```
Banana:
   Calories: 105
   Fat: 0.4 g
   Carbs: 27 g

Grapes:
   Calories: 62
   Fat: 0.3 g
   Carbs: 16 g
```

```
Fruits: <- Element
    -    Banana: <- List
			 Calories: 105 <- Dictionary
			 Fat: 0.4 g
			 Carbs: 27g

    -    Grape: <- List
             Calories: 62 <- Dictionary
             Fat: 0.3 g
             Carbs: 16 g
```

- 

![yamldictionary.png](Attachments/yamldictionary.png)

- Dictionary within a Dictionary

![yamldictionarywithindictionary.png](Attachments/yamldictionarywithindictionary.png)

- Dictionary within an Array

![yamlarraydictionary.png](Attachments/yamlarraydictionary.png)

![listofdictionaries.png](Attachments/listofdictionaries.png)

- Array within a Dictionary

![yamldictionaryarray.png](Attachments/yamldictionaryarray.png)

![yamldictionaryarray-2.png](Attachments/yamldictionaryarray-2.png)


---



