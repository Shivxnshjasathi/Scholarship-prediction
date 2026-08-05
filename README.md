🎓 AI Scholarship FinderWelcome to the AI Scholarship Finder! This project makes it easy for students to find scholarships they actually qualify for, using Artificial Intelligence to predict their chances of winning.🚀 What Does This Project Do?Searching for scholarships is often overwhelming and confusing. This tool does the hard work for you in two main steps:Smart Filter: It instantly removes scholarships you don't qualify for based on your degree, gender, and community.AI Ranking: Instead of giving you a random list, it uses AI to score the remaining scholarships and puts the ones you are most likely to win at the very top!🗺️ How It Works (Visualized)Here is a simple look at how the system processes your information behind the scenes:graph TD
    A[👨‍🎓 You Enter Your Details \n e.g., PG, Female, General] --> B{1. The Basic Filter}
    B -->|Removes non-matches| C[📋 List of Eligible Scholarships]
    
    D[(Past Scholarship Data)] --> E[🤖 AI Learns the Rules]
    E -.->|Applies rules| F
    
    C --> F{2. AI Calculates Confidence Score}
    F --> G[⭐ Final Ranked List \n Highest chances at the top!]
(You can replace the text below with an actual image of your flowchart/process if you have one!)![System Workflow](insert_image_link_here.jpg)🧠 Why We Chose the Decision Tree ModelFor the AI brain, we used a Machine Learning model called a Decision Tree. Here is why it is the perfect fit for this project:It thinks like a human: Scholarship committees use strict rules (e.g., "If Marks > 80% AND Income < 2 Lakhs -> Eligible"). Decision Trees learn by making exactly these kinds of "Yes/No" flowcharts.Easy to understand: Unlike some AI that acts like a hidden magic box, we can actually look at a Decision Tree and see exactly why it gave you a high score.High Accuracy: It performs incredibly well on structured table data (like our Excel sheet). Our model achieved an impressive 86.69% accuracy!(Place a screenshot of your model's 86.69% accuracy output here)![Model Accuracy](insert_image_link_here.jpg)💻 Code Walkthrough (Step-by-Step)Here is a simple explanation of what each part of our Python code is actually doing.Step 1: Loading the DataWhat it does: We use a tool called pandas to open our giant Excel file containing thousands of scholarships. We then print the first few rows so we can make sure the data loaded correctly.import pandas as pd
file_path = '/content/dataset_combined.xlsx'
df = pd.read_excel(file_path)
display(df.head())
(Place a screenshot of the dataset preview table here)![Dataset View](insert_image_link_here.jpg)Step 2: Training the AIWhat it does:Translation: AI only reads numbers, not text. We use LabelEncoder to translate words like "Female" or "Undergraduate" into numbers (like 0 and 1).Studying: We give the AI 80% of our past data to study and learn the scholarship rules.Testing: We test the AI on the remaining 20% to see how smart it got (which is where we got our 86.69% score!).# Select important columns to look at
features = ['Education Qualification', 'Gender', 'Community', 'Religion', 'Annual-Percentage', 'Income']

# Translate text words into numbers
for col in features:
    le = LabelEncoder()
    ml_df[col] = le.fit_transform(ml_df[col].astype(str))

# Train the Decision Tree Model
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
model = DecisionTreeClassifier(max_depth=10, random_state=42)
model.fit(X_train, y_train)
Step 3: The Search & Rank EngineWhat it does: This is the core engine of the app. It takes the user's profile, throws away scholarships they don't match using filters, and then asks the trained AI to calculate a percentage score (AI_Confidence) for all the ones left. It then sorts them highest to lowest.def get_all_applicable_scholarships(user_profile):
    # ... (Code that filters out non-matching scholarships) ...

    # Ask AI to rank the remaining valid scholarships
    if not final_matches.empty:
        probs = model.predict_proba(ranking_data)[:, 1] # Get probabilities
        final_matches.loc[:, 'AI_Confidence'] = (probs * 100).round(2) # Convert to %
        final_matches = final_matches.sort_values(by='AI_Confidence', ascending=False)

    return final_matches
Step 4: The User Interface (Google Colab)What it does: This creates the neat dropdown menus you click on in Google Colab. You pick your details (like "PG" and "Female"), it sends that to the search engine from Step 3, and beautifully prints out a clean table of your top 15 highest-ranked scholarship recommendations!# Get user inputs from interactive dropdowns
edu = "PG" #@param ["10th", "12th", "UG", "PG"]
gen = "Female" #@param ["Male", "Female", "All"]
# ...

# Run the search engine
all_matches = get_all_applicable_scholarships(profile)

# Show the top 15 results in a neat table
display_df = all_matches.drop_duplicates(subset=['Name']).head(15)
display(display_df[['Name', 'Education Qualification', 'Community', 'Gender', 'AI_Confidence']])
(Place a screenshot of the final output table showing the AI Confidence scores here)![Final Recommendations Table](insert_image_link_here.jpg)
