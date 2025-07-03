# adithya-aiml-educational
AI&amp;ML implementing in edudational sector

import pandas as pd

# Upload your CSV file
from google.colab import files
uploaded = files.upload()

# Read the uploaded file
filename = list(uploaded.keys())[0]
df = pd.read_csv(filename)

# Assuming the CSV has these columns: Name, ENGLISH, MATHS, SCIENCE, SOCIAL, COMPUTER

# Calculate average
df['Average'] = df[['English', 'Maths', 'Science', 'Social', 'Computer']].mean(axis=1)

# Determine pass/fail: if any subject < 35 -> Fail
df['Status'] = df[['English', 'Maths', 'Science', 'Social', 'Computer']].apply(lambda row: 'Fail' if any(m < 35 for m in row) else 'Pass', axis=1)

# Provide suggestion
def suggest_class(avg, status):
    if avg > 80:
        return 'No need of extra class'
    elif avg < 50 or status == 'Fail':
        return 'Need of extra class'
    else:
        return 'Optional'

df['Suggestion'] = df.apply(lambda row: suggest_class(row['Average'], row['Status']), axis=1)

# Display the final dataframe
print(df[['Name', 'Average', 'Status', 'Suggestion']])

# Step 6: Save the full result with status
output_file = "students_with_extra_class_status.csv"
df.to_csv(output_file, index=False)

# Step 7: Download the result
files.download(output_file)
