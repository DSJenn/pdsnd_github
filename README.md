>**Note**: Please **fork** the current Udacity repository so that you will have a **remote** repository in **your** Github account. Clone the remote repository to your local machine. Later, as a part of the project "Post your Work on Github", you will push your proposed changes to the remote repository in your Github account.

### Date created
Include the date you created this project and README file.

### Project Title
Replace the Project Title

### Description
Describe what your project is about and what it does

### Files used
Include the files used

### Credits
It's important to give proper credit. Add links to any repo that inspired you or blogposts you consulted.

### Code
import time
import pandas as pd
import numpy as np
import datetime

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    print('Hello! Let\'s explore some US bikeshare data!')
    while True:
        cities = ['chicago', 'new york city', 'washington']
        city = input('Would you like to see data for Chicago, New York City or Washington?\n').lower()
        if city in cities:
            break
        else: 
            print('Invalid input!\n')
    while True:
        months = ['january', 'february', 'march', 'april', 'may', 'june', 'all']
        month = input("\nWhich month would you like to filter by?\nType 'all' if you don't want to filter by month.\n").lower()
        if month in months:
            break
        else:
            print('Invalid input\n')
    while True:
        days = ['sunday', 'monday', 'tuesday', 'wednesday', 'thrusday', 'friday', 'saturday']
        day = input("Which day would you like to see data for?\n").lower()
        if day in days:
            break
        else: 
            print("Invalid input\n")
        
    print('-'*40)
    return city, month, day

def load_data(city, month, day):
    df = pd.read_csv(CITY_DATA[city])
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.weekday_name
    df['hour'] = df['Start Time'].dt.hour
    
    if month != 'all':
        months = ['january', 'february', 'march', 'april', 'may', 'june', 'july', 'august', 'september', 'october','november', 'december']
        month = months.index(month) +1
        df = df[df['month'] == month]
    
    if day != 'all':
        df = df[df['day_of_week'] == day.title()]

    return df

def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    comm_mth = df['month'].mode()[0]
    print('Most common month: {}'.format(comm_mth))

    comm_dow = df['day_of_week'].mode()[0]
    print('Most common day of week: {}'.format(comm_dow))
    
    comm_hour = df['hour'].mode()[0]
    print('Most common start hour: {}'.format(comm_hour))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    comm_start = df['Start Station'].mode()[0] 
    print("The most commonly used start station: {}".format(comm_start))
    
    comm_end = df['End Station'].mode()[0]
    print("The most commonly used end station: {}".format(comm_end))

    station and end station trip
    popular_trip = df.groupby(['Start Station', 'End Station']).agg(lambda x:x.value_counts().index[0])
    print("The most popular trip from start to end is {}".format(popular_trip))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    total_time = df['Trip Duration'].sum()
    print('Total travel time: {}'.format(str(datetime.timedelta(seconds = total_time))))
    
    mean_time = df['Trip Duration'].mean()
    print('Average travel time: {}'.format(str(datetime.timedelta(seconds= mean_time))))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    user_types = df['User Type'].value_counts()
    print(user_types)

    if city != 'washington':
        gender_count = df['Gender'].value_counts()
        print("\nThe counts grouped by users' gender is: {}".format(gender_count))
    else: 
        print ("There is not gender data for this city.")

    if city != 'washington':
        earliest_bday = df['Birth Year'].min()
        recent_bday = df['Birth Year'].max()
        comm_bday = df['Birth Year'].mode()[0]
        print("\nEarliest year of birth: {}, Most recent year of birth: {}, Most common year of birth: {}\n".format(earliest_bday, recent_bday, comm_bday))
    else: 
        print("\nThere is no birth data for this city. Please enter Chicago or New York City, instead.\n")


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break

if __name__ == "__main__":
	main()