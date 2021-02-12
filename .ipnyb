# sentimental-analysis
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import re
import csv
from textblob import TextBlob
import tweepy
import sys
class SantimentalAnalysis:
    def __init__(self):
        self.tweets = []
        self.tweet_text = []
    def Download(self):
        consumer_key = '007nqNdbx5Fbln7Q9yhJbrngi' 
        consumer_secret = '8qluPI4ccQRxvsIjBIGdmAk5vRVHYrPrjnkeV9dUFaklOlMglh'
        access_token = '1151519439902220289-0PeBNcTImyw2SIw8HwZAEF2SWqPnbG'
        access_token_secret = 'uH4ABzbfQpPAAfZPlBedXInumT6fmeKCxC2na7icdH9cU' 
        authentication = tweepy.OAuthHandler(consumer_key,consumer_secret)
        authentication.set_access_token(access_token,access_token_secret)
        api = tweepy.API(authentication)
        return api
    
    def fetch_tweetes(self,search_term,no_of_term,api):
         #search for tweets
        self.tweets = tweepy.Cursor(api.search,q = search_term,lang = "en").items(no_of_term)
        
        #open and creat a csv file to append tha data
        csvfile = open('result.csv','a')
        csvWriter = csv.writer(csvfile)
        #declayred some variable to store some inforamtion 
        polarity = 0
        positive = 0
        wpositive = 0
        spositive = 0
        negative = 0
        wnegative = 0
        snegative = 0
        neutral = 0
       
        
        for tweet in self.tweets:
                      #Append to temp so that we can store in csv later. I use encode UTF-8
           # print(tweet)          
            self.tweet_text.append(self.cleantweet(tweet.text).encode('utf-8'))
                #print (tweet.text.translate(non_bmp_map))    #print tweet's text
            analysis = TextBlob(tweet.text)
            #print(analysis.sentiment)  # print tweet's polarity
            polarity += analysis.sentiment.polarity  # adding up polarities to find the average later
                
            if (analysis.sentiment.polarity == 0):  # adding reaction of how people are reacting to find average later
                    neutral += 1
            elif (analysis.sentiment.polarity > 0 and analysis.sentiment.polarity <= 0.3):
                    wpositive += 1
            elif (analysis.sentiment.polarity > 0.3 and analysis.sentiment.polarity <= 0.6):
                    positive += 1
            elif (analysis.sentiment.polarity > 0.6 and analysis.sentiment.polarity <= 1):
                    spositive += 1
            elif (analysis.sentiment.polarity > -0.3 and analysis.sentiment.polarity <= 0):
                    wnegative += 1
            elif (analysis.sentiment.polarity > -0.6 and analysis.sentiment.polarity <= -0.3):
                    negative += 1
            elif (analysis.sentiment.polarity > -1 and analysis.sentiment.polarity <= -0.6):
                    snegative += 1
        #write tweets_text into csv file and close csv file
        csvWriter.writerow(self.tweet_text)
        csvfile.close()
        #to find out percentage 
        positive = self.percentage(positive,no_of_term)
        spositive = self.percentage(spositive,no_of_term)
        wpositive = self.percentage(wpositive,no_of_term)
        negative = self.percentage(negative,no_of_term)
        snegative = self.percentage(snegative,no_of_term)
        wnegative = self.percentage(wnegative,no_of_term)
        neutral = self.percentage(neutral,no_of_term)
        
        #to findout avrage
        polarity = polarity/no_of_term
        print("How people are reacting on " + search_term + " by analyzing " + str(no_of_term) + " tweets.")
        print()
        print("General Report: ")
        
        if polarity == 0:
            print("Neutral")
        elif polarity > 0 and polarity <=0.3:
            print("Weakly positve")
        elif polarity >0.3 and polarity <=0.6:
            print("positive")
        elif polarity > 0.6 and polarity <=1:
            print("storngly positive")
        elif polarity >-0.3 and polarity <= 0:
            print("weakly negative")
        elif polarity > -0.6 and polarity <= -0.3:
            print("Negative")
        elif polarity > -1 and polarity <=-0.6:
            print("stongly negative")
        
        
        print()
        print("Detailed Report: ")
        print(str(positive) + "% people thought it was positive")
        print(str(wpositive) + "% people thought it was weakly positive")
        print(str(spositive) + "% people thought it was strongly positive")
        print(str(negative) + "% people thought it was negative")
        print(str(wnegative) + "% people thought it was weakly negative")
        print(str(snegative) + "% people thought it was strongly negative")
        print(str(neutral) + "% people thought it was neutral")

        self.plotPieChart(positive,wpositive,spositive,negative,wnegative,snegative,neutral,search_term,no_of_term)
        # to remove special character ,link  from tweet
    def cleantweet(self,tweet):
            #print(tweet)
            #print(' '.join(re.sub("(@[A-Za-z0-9]+)|([^0-9A-Za-z \t]) | (\w +:\ / \ / \S +)", " ", tweet).split()))
            return ' '.join(re.sub("(@[A-Za-z0-9]+)|([^0-9A-Za-z \t]) | (\w +:\ / \ / \S +)", " ", tweet).split())
        # to find percentage
    def percentage(self,part,whole):
            temp = 100*float(part)/float(whole)           
            return format(temp,'.2f')
        #to show chart
    def plotPieChart(self, positive, wpositive, spositive, negative, wnegative, snegative, neutral, searchTerm, noOfSearchTerms):
            labels = ['Positive [' + str(positive) + '%]', 'Weakly Positive [' + str(wpositive) + '%]','Strongly Positive [' + str(spositive) + '%]', 'Neutral [' + str(neutral) + '%]',
                      'Negative [' + str(negative) + '%]', 'Weakly Negative [' + str(wnegative) + '%]', 'Strongly Negative [' + str(snegative) + '%]']
            sizes = [positive, wpositive, spositive, neutral, negative, wnegative, snegative]
            colors = ['yellowgreen','lightgreen','darkgreen', 'gold', 'red','lightsalmon','darkred']
            patches, texts = plt.pie(sizes, colors=colors, startangle=90)
            plt.legend(patches, labels, loc="best")
            plt.title('How people are reacting on ' + searchTerm + ' by analyzing ' + str(noOfSearchTerms) + ' Tweets.')
            plt.axis('equal')
            plt.tight_layout()
            plt.show()  
                
            
if __name__ == "__main__":      
    sant  = SantimentalAnalysis()
    api = sant.Download()  
    print(api)
    search_term = input("Enter Keyword/Tag to search about:")
    no_of_term = int(input("enter howmany tweets you wants to search"))
    sant.fetch_tweetes(search_term,no_of_term,api)
    
