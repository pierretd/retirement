import dash
import dash_core_components as dcc
import dash_html_components as html
import pandas as pd
import plotly.graph_objs as go

df = pd.read_csv(
    'https://raw.githubusercontent.com/pierretd/data-dump/master/finance/topology/all%20states%20-%20Sheet1.csv')

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

app.layout = html.Div([
    dcc.Graph(id='graph-with-slider'),
    dcc.Slider(
        id='Year-slider',
        min=df['Year'].min(),
        max=df['Year'].max(),
        value=df['Year'].min(),
        marks={str(Year): str(Year) for Year in df['Year'].unique()}
    )
])


@app.callback(
    dash.dependencies.Output('graph-with-slider', 'figure'),
    [dash.dependencies.Input('Year-slider', 'value')])
def update_figure(selected_Year):
    filtered_df = df[df.Year == selected_Year]
    traces = []
    for i in filtered_df.Region.unique():
        df_by_Region = filtered_df[filtered_df['Region'] == i]
        traces.append(go.Scatter(
            x=df_by_Region['RETTP79'],
            y=df_by_Region['POVTP79'],
            text=df_by_Region['State'],
            mode='markers',
            opacity=0.8,
            marker={
                'size': 15,
                'line': {'width': 0.5, 'color': 'white'}
            },
            name=i
        ))

    return {
        'data': traces,
        'layout': go.Layout(
            xaxis={'type': 'log', 'title': 'Retirement'},
            yaxis={'title': 'Poverty', 'range': [0, 10]},
            margin={'l': 40, 'b': 40, 't': 10, 'r': 10},
            legend={'x': 0, 'y': 1},
            hovermode='closest'
        )
    }


if __name__ == '__main__':
    app.run_server(debug=True)
