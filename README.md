# IntroDataScience

Prompt list:
 have a csv file called card_transdata, import that as well as the necessary libraries like seaborn, matplotlib, and pandas
 
I want you to do these things in my current notebook file


check for missing values

go back to the import cell and import numpy

git rid of the heatmap

drop rows with missing values

give me a countplot of the fraud column

also give me count plots for Used Pin and Repeat Retailer

it should be used_pin_number

I want to see them individually, not all three next to each other, it makes them too small

do one for Online orders

make a histogram of the ratio to median purchase price

make a bivariate countplot of the pin usage with hue based on if it was fraud

Create a percentage plot to show the percentage of fraudulent transactions when a PIN was used or not

this is close, but the percentages should add up to 100% and show percentages that weren't fraud. It should be a stacked bar chart and have a hue 
difference to show fraud

repeat the last two charts but with online orders. And make the charts with less lines of code

Create a scatter plot to show the breakdown of fraudulent and non-fraudulent transactions for distance_from_last_transaction vs 
ratio_to_median_purchase_price

Create a scatter plot to show the breakdown of fraudulent and non-fraudulent transactions
okay perfect, but make the last two plots less transparent


AI System: The auto-ai in Visual Studio Code, which I believe also entirely used Claude Sonnet 3.5

Reflection:
Using an ai waas defintely helpful in the sense that it was time saving, but it is defintely a dangerous thing to rely on for someone who does not actually know data science. While the AI was able to produce the correct charts, it had a tendency to use way more code (and more inefficient code) than necessary. It seemed to serverly lack knowledge in Pandas. For example, to produce practically the same chart, the code we used was 




>>>
df_pin_fraud = transaction_data_cleaned.groupby('used_pin_number')['fraud'].value_counts(normalize=True).unstack() * 100

df_pin_fraud.plot(kind='bar', stacked=True, color=['green', 'red'], figsize=(5, 3))
plt.title('Percentage of Fraudulent Transactions with PIN Usage')
plt.xlabel('Used PIN')
plt.ylabel('Percentage')
plt.legend(title='Fraud', labels=['Non-Fraudulent', 'Fraudulent'])
plt.show().

>>>




The AI produced the following code to get the same output:





counts = df.groupby(['used_pin_number', 'fraud']).size().unstack(fill_value=0)

for col in [0, 1]:
    if col not in counts.columns:
        counts[col] = 0
counts = counts[[0, 1]]  # columns: 0 = not fraud, 1 = fraud

percent = counts.div(counts.sum(axis=1), axis=0) * 100

labels = ['No' if int(x) == 0 else 'Yes' for x in percent.index]

fig, ax = plt.subplots(figsize=(8, 6))
bottom = np.zeros(len(percent))
colors = ['#4c72b0', '#c44e52']  # color for not fraud, fraud
for i, col in enumerate(percent.columns):
    vals = percent[col].values
    bars = ax.bar(labels, vals, bottom=bottom, color=colors[i], label=('Not Fraud' if col == 0 else 'Fraud'))
    # Annotate each segment if visible
    for bar, v in zip(bars, vals):
        if v >= 1:
            ax.text(bar.get_x() + bar.get_width() / 2,
                    bar.get_y() + bar.get_height() / 2,
                    f"{v:.1f}%",
                    ha='center', va='center', color='white', fontsize=10)
    bottom += vals

ax.set_ylim(0, 100)
ax.set_ylabel('Percentage of Transactions (%)')
ax.set_title('Percentage of Fraudulent vs Non-Fraudulent Transactions by PIN Usage')
ax.legend(title='Transaction Type')
plt.show()



It is much longer, less readable, and less efficient. One of the first things we learn in this class is that we really shouldn't be using for loops, and the AI uses nested for loops to create this chart. 
