# order_sales_analysis-Data_analytics-
#to extarct the dataset from kaggle
!pip install kaggle
import kaggle
!kaggle datasets download -d ankitbansal06/retail-orders --unzip

#to clean the data by droping unknown values
import pandas as pd
df=pd.read_csv('orders.csv',na_values=[ 'Not Available', 'unknown'])
df['Ship Mode'].unique()

#to make all the chracters into lower case and replace the space with (_) character
df.columns=df.columns.str.lower()
df.columns=df.columns.str.replace(' ','_')
df.columns

#to create the another column disocunt
df['discount']=df['list_price']*df['quantity']*.01
df.columns
df['sale_price']=df['list_price']-df['discount']
df['profit']=df['sale_price']-df['cost_price']
df
#to create the connection between mysql and jupyter notebook
from sqlalchemy import create_engine
engine = create_engine('mysql+pymysql://root:root@localhost:3306/orders')
df.to_sql('df_orders', con=engine, if_exists='append', index=False)
