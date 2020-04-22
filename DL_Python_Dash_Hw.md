```python
import dash
import dash_core_components as dcc
import dash_html_components as html
import pandas as pd
import plotly.express as px

# Uses an External Stylesheet
# Use a css file from your GitHub Pages site 
external_stylesheets = ['https://usfmumaanalyticsteam.github.io/learn.css']

# Creates the app to instantiate the content for the Dashboard and use the external_stylesheets
app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

# Use a csv dataset from a repository in your GitHub account. Use the Raw URL to expose the data to the Python dataframe
df = pd.read_csv('https://raw.githubusercontent.com/USFMumaAnalyticsTeam/data/master/gdp-life-exp-2007.csv')
#df2 = pd.read_csv('https://raw.githubusercontent.com/USFMumaAnalyticsTeam/data/master/usa-agricultural-exports-2011.csv')
df2 = pd.read_csv('https://raw.githubusercontent.com/Davidlam1990/data/master/Covid_FL.csv')



# Custom function used to generate a data table from a dataframe
def generate_table(dataframe, max_rows=16):
    return html.Table([
        html.Thead(
            html.Tr([html.Th(col) for col in dataframe.columns])
        ),
        html.Tbody([
            html.Tr([
                html.Td(dataframe.iloc[i][col]) for col in dataframe.columns
            ]) for i in range(min(len(dataframe), max_rows))
        ])
    ])


# Add content to the app layout
# Begin all content DIV
app.layout=html.Div([
    # Add your HTML tags to the content - notice a comma is added between HTML elements
    html.H1('Welcome To Data Visualization Page'),
    html.Div([
        html.P('Top News of The Day ...'),
    ]),
    # Begin of DIV surrounding both Tables
    html.Div([
    # Begin of First Table
    html.Table(style={'width':'100%'},
               # Begin of Table children
               children=[
                   #######################################################################
                   # Begin of First Tr
                 html.Tr(
                     #Begin Tr children
                     children=[
                         # Begin Th
                         
                         html.Th(style={'width':'30%'},
                             # Begin Th children
                             children=[
                                 html.H3('Florida Covid-19 Report')
                             # End of Th children   
                             ]
                         
                         # End of Th - Notice a comma is placed here to separate the next Th
                         ),
                         # Begin of Th
                         html.Th(style={'width':'70%'},
                             # Begin of Th children
                             children=[
                                 html.H3('Global GDP for Fruits and Veggies')
                             # End of Th children    
                             ]
                         
                         # End of Th
                         )
                         
                     # End of Tr children    
                     ]
                 # End of First Tr - Notice a comma is placed here to separate the next Tr
                 ),
                 #########################################################################
                 # Begin of Second Tr
                 html.Tr(
                     #Begin Tr children
                     children=[
                         # Begin Td
                        html.Td(
                             # Begin of Td children
                             children=[
                                  dcc.Graph(
                                    id='example-graph',
                                    figure={
                                        'data': [
                                            {'x': df2['Combined_Key'], 'y': df2['Deaths'], 'type': 'bar', 'name': 'SF'}
                                        ],
                                        'layout': {
                                            'title': 'Top 10 deaths in FL by County on 4/1/2020'
                                        }
                                    }
                                ) 
                             ]
                         # End of Td
                         ),
                         # Being of Td
                         html.Td(
                             # Begin of Td children
                             children=[
                                 # Display plot graph use data from dataframe df
                                 dcc.Graph(
                                    id='life-exp-vs-gdp',
                                figure={
                                    'data': [
                                        dict(
                                            x=df[df['continent'] == i]['gdp per capita'],
                                            y=df[df['continent'] == i]['life expectancy'],
                                            text=df[df['continent'] == i]['country'],
                                            mode='markers',
                                            opacity=0.7,
                                            marker={
                                                'size': 15,
                                                'line': {'width': 0.5, 'color': 'white'}
                                            },
                                            name=i
                                        ) for i in df.continent.unique()
                                    ],
                                    'layout': dict(
                                        xaxis={'type': 'log', 'title': 'GDP Per Capita'},
                                        yaxis={'title': 'Life Expectancy'},
                                        margin={'l': 40, 'b': 40, 't': 10, 'r': 10},
                                        legend={'x': 0, 'y': 1},
                                        hovermode='closest'
                                    ), 'layout': {
                                            'title': 'GDP by Continent'}
                                    # End of 2nd Inner DIV
                                    })
                           
                             # End of Td children    
                             ]
                         # End of Td
                         )
                     # End of Tr children    
                     ]
                 # End of Tr
                 )     
                #########################################################################                   
               #End of Table Children    
               ]
              # End of First Table - Notice a comma is placed here to separate the next Table
              ),
    
    #########################################################################################   
    # Begin of Second Table
    html.Table(style={'width':'100%'},
               children=[
                 #######################################################################
                 # Begin First Tr
                 html.Tr(
                     #Begin Tr children
                     children=[
                         # Begin Th
                         html.Th(style={'width':'30%'},
                             # Begin Th children
                             children=[
                                 html.H3('2019 Impact of Exports on US GDP')
                             # End of Th children   
                             ]
                         
                         # End of Th - Notice a comma is placed here to separate the next Th
                         ),
                         #Begin of Th
                         html.Th(style={'width':'10%'},
                             # Begin of Th children
                                 children=[
                                 # Nothing to display here, just a place holder in the column
                                 html.H2('')
                                 # End of Th children    
                                 ]
                             # End of Th - Notice a comma is placed here to separate the next Th
                         ),
                         # Begin of Th
                         html.Th(style={'width':'70%'},
                             # Begin of Th children
                             children=[        
                                    html.H3('Top 15 deaths in FL by County on 4/1/2020')
                                                               
                             # End of Th children    
                             ]
                         # End of Th
                         )
                     # End of Tr children    
                     ]
                 # End of First Tr
                 ),
                   
                ######################################################################
                # Begin of Second Tr
                 html.Tr(
                     #Begin Tr children
                     children=[
                         # Begin Td
                         html.Td(
                             # Begin Td children
                             children=[
                                    # Display a simple line graph
                                    dcc.Graph(
                                        id='g1', 
                                        figure={'data': [{'y': [1, 2, 3]}], 
                                                'layout': {'title': 'Impact on US GDP'}})
                             # End of Td children   
                             ]
                         
                         # End of Td - Notice a comma is placed here to separate the next Th
                         ),
                         
                         # Begin of Td
                         html.Td(style={'border':'1px solid black'},
                             # Begin of Td children
                             children=[
                                 # Display a large and important measure
                                 html.H2('Domestic'),
                                 html.H2('Exports:'),
                                 html.H2('80%')
                             # End of Td children    
                             ]
                         # End of Td
                         ),
                         
                         # Begin of Td
                         html.Td(
                             # Begin of Td children
                             children=[
                                    # Execute custom generate_table function and display data
                                    # Use data from dataframe df2
                                    generate_table(df2)
                             # End of Td children    
                             ]
                         # End of Td
                         )
                     # End of Tr children    
                     ]
                 # End of Second Tr
                 )     
               
               #######################################################################
               #End of Table Children    
               ]
              #########################################################################################
              # End of Second Table - Notice a comma is placed here to separate the next Content
              ),
    # End of DIV surrounding both Tables
    ]),
               
# End of all content DIV
])

# Run the app on the web server
if __name__ == '__main__':
    # Set debug to False. Some configurations are not setup for Debug
    app.run_server(debug=False)
```

     * Serving Flask app "__main__" (lazy loading)
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
    

     * Running on http://127.0.0.1:8050/ (Press CTRL+C to quit)
    127.0.0.1 - - [22/Apr/2020 10:02:10] "GET / HTTP/1.1" 200 -
    127.0.0.1 - - [22/Apr/2020 10:02:10] "GET /_dash-dependencies HTTP/1.1" 200 -
    127.0.0.1 - - [22/Apr/2020 10:02:10] "GET /_dash-layout HTTP/1.1" 200 -
    


```python

```
