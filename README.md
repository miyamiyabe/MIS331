# MIS331

import pandas as pd
import csv

def load_currency_codes_from_google_sheet(google_sheet_id):
    # Construct the API link
    api_link = f"https://docs.google.com/spreadsheets/d/{google_sheet_id}/gviz/tq?tqx=out:csv"

    # Read the data from the API link into a pandas dataframe
    df = pd.read_csv(api_link)

    # Convert the dataframe to a dictionary
    currency_codes = dict(zip(df['CountryName'], df['Code']))

    return currency_codes

def get_currency_code(country_name, currency_codes):
    return currency_codes.get(country_name, "Not found")

# Google Sheets ID
google_sheet_id = "1apuhth4peoyK-UbQY5xTvvwdMatdkZPD5OUBLNjIlYE"

# Load currency codes from Google Sheets
currency_codes = load_currency_codes_from_google_sheet(google_sheet_id)

from_country_name = input("Enter the country name that you want to convert from: ")

from_country_code = get_currency_code(from_country_name, currency_codes)

if from_country_code == "Not found":
    print(f"Currency code not found. Please type the common name of the country. Ex: U.S. United States")
    from_country_name = input("Enter the country name that you want to convert from: ")
    from_country_code = get_currency_code(from_country_name, currency_codes)

to_country_name = input("Enter the country name that you want to convert to: ")

to_country_code = get_currency_code(to_country_name, currency_codes)

if to_country_code == "Not found":
    print(f"Currency code not found. Please type the common name of the country. Ex: U.S. United States")
    to_country_name = input("Enter the country name that you want to convert to: ")
    to_country_code = get_currency_code(to_country_name, currency_codes)



  #CurrencyCode Converter

import requests
url = 'http://api.exchangeratesapi.io/v1/latest?access_key=96db186baf3a08d762372b34f9226864'

#Define a function to handle currency conversion
def currency_conversion():
    #Prompt user to imput source currency
    from_currency = from_country_code
    #Promt user to input target currency
    to_currency = to_country_code
    #Promt user to input amount to convert
    amount = int(input('Amount: '))

    #Send an HTTP GET request to the API endpoint
    response = requests.get(url)
    #Extract exchange rate for source currency
    rate = response.json()['rates'][from_currency]

    #Convert amount to equivalent amount in Euros
    amount_in_EUR = amount/rate
    #Convert amount to target currency
    new_amount = amount_in_EUR * (response.json()['rates'][to_currency])

    #Round converted amount to two decimal
    new_amount = round(new_amount,2)
    #write a statement
    print(f"{amount} {from_currency} is {new_amount} {to_currency}")

#Call the function to start the currency conversion
currency_conversion()
