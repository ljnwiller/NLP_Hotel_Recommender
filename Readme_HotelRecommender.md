# Hotel Recommender System

> Jianuo Liu, Rai Gohalwar

## Background

We aimed to build a hotel recommender system for either regular users based on existing history information or new users based on reviews of hotels. 

## Install and Configuration

This recommender system takes advantage of [Surprise](https://surprise.readthedocs.io/en/stable/index.html#) library. Before importing the Surprise library, we need to install it on Google Colab.

```python
pip install surprise
```

The dataset used in this project called [Hotel Reviews](https://www.kaggle.com/datafiniti/hotel-reviews), and it can be accessed through kaggle.

In order to download and use kaggle data within Google Colab:

1) install the kaggle:

```python
! pip install -q kaggle
```

2) Download kaggle.json file by clicking on **Create New API Token** in your own kaggle account

3) Upload kaggle.json file

```python
from google.colab import files
files.upload()
```

4) Make directory named kaggle and copy kaggle.json file there

```
! mkdir ~/.kaggle
! cp kaggle.json ~/.kaggle/
```

5) Change the permission of the file

```
! chmod 600 ~/.kaggle/kaggle.json
```

6) Download the dataset by using its API command which can be obtained (copied) on kaggle

```python
! kaggle datasets download -d datafiniti/hotel-reviews
```

7)  Use unzip command yo unzip the data

```python
! unzip hotel-reviews.zip
```

8) Read the .csv file chosen to use in this project, and we use *Datafiniti_Hotel_Reviews_Jun19.csv*

```python
reviews_jun = pd.read_csv('/content/Datafiniti_Hotel_Reviews_Jun19.csv')
```



## User Interface

The user interface is based on the [ipywidgets](https://ipywidgets.readthedocs.io/en/latest/) library commonly used with jupyter notebooks and Google Colab. Before importing the ipywidgets library, we need to install it in Google Colab.

```python
!pip install ipywidgets
```

The the downloaded library is imported into the workspace along with the display function from IPython. 

```python
import ipywidgets as widgets
from IPython.display import display
```

Creating the interface environment uses the following steps:

1. Set-up margins for each element in UI

   ```
   item_layout = widgets.Layout(margin='0 50px 50px 50px')
   ```

   

2. Show welcome message

   ```
   title = widgets.Label(value="Welcome to the Hotel Recommendation System!",layout=item_layout )
   ```

   

3. Toggle button for recommendation method

   ```
   method_title = widgets.Label(value="Choose a recommendation method:",layout=item_layout )
   
   type_of_method = widgets.ToggleButtons(options=["Review Based", "ID Based"], value= "ID Based", layout=item_layout)
   ```

   

4. User ID input 

   ```
   id_title = widgets.Label(value="If using ID Based method, input your USER ID\n\n",layout=item_layout )
   
   user_id_input = widgets.Text(description="USER ID : ", width=200, layout=item_layout, value='47')
   ```

   

5. City selection dropdown

   ```
   places = reviews_jun['city']+"," + reviews_jun['province']
   
   places = places.drop_duplicates().sort_values().tolist()
   
   
   
   city_input = widgets.Dropdown(options=places, value=places[0],description="Select city:", layout=item_layout)
   ```

   

6. Slider for # of output hotels

   ```
   n_outputs = widgets.IntSlider(value=1,min=1,max=5,step=1,description="Hotels:", layout=item_layout)
   ```

   

7. Search button

   ```
   search_button = widgets.Button(description="SEARCH")
   ```

   

8. Button click handler, prints input parameters and processes input variables based on method selected

   ```python
   def search_clicked(change):
    output.clear_output()
    with output:
     print("Input Parameters")
     print("method: %s" % type_of_method.value)
     print("user id: %s" % user_id_input.value)
     input_city,input_province = city_input.value.split(",")
     print("city: %s" % input_city)
     print("province: %s" % input_province)
     print("number of hotels: %s" % n_outputs.value)
   
     if type_of_method.value == "ID Based":
      \# code to execute of ID based chosen
      \# code to execute of ID based chosen
      recommended_hotels = recommendate_topn_hotels_for_user(reviews_jun, 			      all_pred_sorted, int(user_id_input.value), input_city, input_province, 		    topn=n_outputs.value)
   
      recommended_hotels = recommended_hotels.set_index("name").drop(columns=         ["primaryCategories","country","reviews.rating","id"]).reindex(columns=   ["address","postalCode","websites","city","province","categories"])
   
      display(recommended_hotels)
   
      
   
     else:
   
      # code to execute of review based chosen
   
      most_popular_filtered =                             filtered_hotels.loc[filtered_hotels["city"]==input_city].loc[filtered_hotels["province"]==input_province].iloc[0:n_outputs.value]
   
      display(most_popular_filtered)
   ```

   

9. display all elements

   ```
   display(title)
   display(method_title)
   display(type_of_method)
   display(id_title)
   display(user_id_input)
   display(city_input)
   display(n_outputs)
   display(search_button)
   output = widgets.Output()
   display(output)
   ```

   

10. link button click to handler

    ```
    1. search_button.on_click(search_clicked)
    ```

