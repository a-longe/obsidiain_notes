[Link](https://ca.prairielearn.com/pl/course_instance/3529/instance_question/67316672/)
# __count_even-odds(file)__
- returns an [dictionary](Dictionaries) with exactly two key:value pairs which are there the number of even and odd numbers
	```python
	even_odd:dict[str:int] = 
	{'Even': 3,
	'Odd': 3}
	```
	
# __get_number_frequencies(file)__
- returns a [dictionary](Dictionaries) that counts the number of times a number is in a text file, the number is the key and the value associated with it is the number of times it appears in the text file
	```python
	number_frequencies:dict[int:int] =
	{1:3,
	2:2,
	6:1,
	4:3}
	```
	
# __most_freq_number__(file)
- returns a list with the most common numbers
	```python
	>>> most_freq_number(number_frequencies)
	[1, 4]
```

# __create_population_dictionaries(file, granularity)__
- Creates two [dictionaries](Dictionaries):
	- One with the name of the country being the key and the exact population being the value
	-  The other dictionary has it's keys created by the population of a country by [[Integer Dividing]]  the granularity the keys are lists of countries that share the same generated key (ie. their population [integer divided](Integer Dividing) by the granularity is the same as other countries are in the same list)

	```python
	# first dictionary
	county_pop:dict[str:int] = {
		'Canada': 3_100_000,
		'Netherlands': 3_700_000,
		'USA': 10_000_000}

	# second dictionary (granularity = 1_000_000)
	pop_country:dict[int:list[str]] ={
		3: ['Canada', 'Netherlands'],
		10: ['USA']}
```

# __population_analysis(file, country, granularity)__
- takes a population data file
- create necessary [dictionaries](Dictionaries) (from create_population_dictionaries)
- returns a list of tuples, sorted by population size of all the countries in the same population list based on `population // granularity`
	
	```python
	'''
	file contents:
	
	San Marino,33203
	Gibraltar,34408
	Monaco,38499
	Turks and Caicos Islands,34900
	'''
	>>> population_analysis(file, 'Gibraltar', 1000)
	[(34408, 'Gibraltar'),
	(34900, 'Turks and Caicos Islands')]
```
	- Turks and Caicos Islands are the only countries with their population // 1000 is the same as Gibraltars