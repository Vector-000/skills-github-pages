---
title: Coca Cola Prices
---
We are going to be comparing the Prices of One litre of Coca Cola around 10 different countries, all in which, Coca Cola is quite famous and heavily consumed. 
The purpose of this study is to get a deeper look into Coca Colas pricing strategy, given that Coca Cola is one of the oldest companies in the world and also has one of the best distribution systems, you would expect the prices to remain the same roughly everywhere, largely unaffected by transport and production costs, however, as we will see later on this is not true.
To begin the analysis, 
Here are the prices in local currency. 
![image](https://github.com/user-attachments/assets/de7a6b07-679e-48f5-9663-8e9ed415aa30)
And here are some additional tables that we will need.
![image](https://github.com/user-attachments/assets/b6af5229-0664-4c5f-8a5b-29c324bf3889)
![image](https://github.com/user-attachments/assets/ea552267-edf3-4829-a7ab-80b1765b77d0)

Finally, we will join all these tables together so we can do some analysis on them.
![image](https://github.com/user-attachments/assets/08981975-76b2-46af-b7bc-04c1c6777f7d)
I then used the following python code to get some extra insight into the data and came up with following table which summarizes everything.
post_tax_prices = final_table.column("Price")*(1+(final_table.column("Average Sales Tax (%)")/100))
final_table = final_table.relabel("Price", "Price Before Tax").with_column("Price After Tax", post_tax_prices)
price_in_CAD = final_table.column("Exchange Rate to CAD")*final_table.column("Price After Tax")
final_table = final_table.with_column("Price in CAD", price_in_CAD)
final_table
![image](https://github.com/user-attachments/assets/36f87c64-db91-4ae1-a73e-b4d77ecdd362)

![image](https://github.com/user-attachments/assets/d49ddf50-5d37-4694-af72-e838de8dbbce)
We will now plot a bar chart of Price in CAD against Country.
![image](https://github.com/user-attachments/assets/f8dd39b0-f679-4838-b473-0fca3d20ff99)
As you can see. The prices of Coca Cola vary greatly across the 10 countries.
My hypothesis is that Coca Cola prices their products in such a way so as to control sales, using GDP per Capita as their metric. To confirm this I decided to scrape a table from the web that contains all the Countries and their GDP per Capita using the following python code.

![image](https://github.com/user-attachments/assets/1e7a1bfc-aab1-4032-90b3-1e18a5cb410a)
![image](https://github.com/user-attachments/assets/dfcaf864-f570-4e72-aa55-9781087e0d2c)
I then proceeded to use some more python code to make the table in a format that has the GDP per Capita in integers and only contains the 10 countries that we need.
def requirements(x):
  if x in final_table.column("Country"):
    return True
  else:
    return False
required_countries = countries_and_gdp.where("Country", requirements)
required_countries
def change_to_int(x):
  x = x.replace("$", "").replace(",","")
  return int(x)
GDP_per_capita_int_and_cad = required_countries.apply(change_to_int ,"GDP Per Capita") * exhange_rates_table.where("Country", "United States")[1]
required_countries = required_countries.drop(1).with_column("GDP Per Capita In CAD", GDP_per_capita_int_and_cad)
![image](https://github.com/user-attachments/assets/8b946f60-eaa4-4c0e-bd1b-81c0d4e994e0)


![image](https://github.com/user-attachments/assets/20714936-cb26-4987-b8a0-9b78753c1ebc)

I then plotted a line graph of the Price in CAD against GDP per Capita for each of the 10 countries.
![image](https://github.com/user-attachments/assets/cabd5653-a4ec-4d82-9b2c-9767a266fd86)
As you can see there is an almost perfect positive correlation between GDP per Capita and Price in CAD. This confirms our hypothesis.

Finally, after taking all the data into account, even though we do see a correlation, this is not enough to accuse Coca Cola on controlling prices, this is because Correlation does not equal Causation and the 2 variables may be linked in some other way, however, this does mean the matter is worth investigating further with data from more countries. 








