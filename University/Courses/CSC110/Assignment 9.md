[Link](https://ca.prairielearn.com/pl/course_instance/3529/instance_question/67577272/)

# __read_file(filename) -> (dict[str, Date], dict[str, list[str]], dict[str, list[str]], dict[str, str])
- Returns 4 [[Dictionaries]] 
	- show_id : date_added
	- show_id : lo_unique_actors
	- category : lo_unique_show_ids
	- show_id : show_title
- If a key is to be added to any dictionary, it _must_ have a corresponding value to be associated with otherwise, do not add the key to the dictionary
- If show_id : lo_unique_actors [dictionary](Dictionary), there is more than one actor with the same name in that list, add the first and do not add any more
	- Note: every time we add to the lo_unique_actors in that [dictionary](Dictionaries), check to make sure that there is not already that name in the list
- Input data should look like on every line
```csv
show_id,type,title,director,cast,country,date_added,
release_year,rating,duration,listed_in,description
```

- date_added, director, listed_in cast should not be strings and must be reformated

- 