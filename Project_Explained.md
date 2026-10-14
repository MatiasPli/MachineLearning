# Lab 1 explained in plain language
**Course:** DA380A Machine Learning · **Dataset:** 5, AirTravel · **Goal:** guess the price of a plane ticket

---

## The big picture
We taught a computer to **guess the price of a plane ticket**. We showed it about 300,000 real flights inside India
(from February and March 2022) together with their prices. It learned the patterns, and then we checked how well it guesses
prices for flights it had never seen.

---

## Step 1: Getting to know the data
Like reading a spreadsheet before using it. For every flight we know:
- the airline, the city it flies from and the city it flies to
- whether it is **business** or **economy** class
- departure and arrival time, how long the trip takes, and how many stops it makes
- how many days before the flight the price was recorded
- **the price** (in Indian rupees), which is what we want to guess

**What we found:**
- **Business class costs about 9 times more** than economy (about 53,000 vs. 5,800 rupees). This matters more than anything else.
- **Booking late is expensive.** An economy ticket bought 1-3 days before the flight costs about 3 times more than one
  bought 3 or more weeks ahead.
- More stops means a higher price, Vistara and Air India are the most expensive airlines, and the day of the week barely matters.
- Some trips take more than 24 hours (long layovers). That is realistic, so we kept them.

## Step 2: Cleaning the data
The data was messy, like a form filled in by hand:
- prices were written as text (`"25,612"`) instead of numbers
- trip durations were written as `"02h 10m"` and had to be turned into minutes
- the "stops" column had invisible junk characters (tabs and line breaks) in it
- 6 rows were broken or duplicated, and we removed them

We also turned words into numbers the computer understands. For example, "Vistara" or "Business" become codes,
because a computer cannot do maths with words.

## Step 3: Hiding a test set
We put **20 %** of the flights in a "locked drawer" and trained only on the other **80 %**. At the very end
we checked the guesses on the locked 20 %.
It's like a teacher keeping the exam questions secret: the student can't just memorise the answers.

## Step 4: Two different "students" (the models)
1. **Linear Regression, the simple student.** It learns simple rules, such as "business adds about 45,000 rupees,
   each extra stop adds a few thousand, each day you book earlier saves about 128 rupees". It is easy to understand, but it
   can only add simple effects together.
2. **Random Forest, the clever student.** It builds hundreds of decision trees (chains of questions like
   "Is it business? Is it Vistara? Was it booked within 3 days?") and averages their answers. It can spot complex
   patterns, for example that booking late raises economy prices much more than business prices.

## Step 5: The exam
We measured how far off the guesses were on the locked test flights:

| | Average miss | How much of the price it explains (R²) |
|---|---|---|
| Simple student (Linear Regression) | about 4,550 rupees | 91 % |
| Clever student (Random Forest) | **about 1,120 rupees** | **98.5 %** |

**Careful:** 91 % sounds great, but it mostly comes from telling business apart from economy. If you look only at
economy tickets, the simple student explained just 8 % of the price differences, while the Random Forest explained 86 %.

The three measures the lab asks for:
- **MAE:** the average miss in rupees.
- **RMSE:** also in rupees, but big misses count extra.
- **R²:** how much of the ups and downs in price the model explains (100 % would be perfect).

## Step 6: Fine-tuning ("tuning")
Each model has settings, like adjusting a recipe. We tried many combinations and scored each one fairly by testing it
5 times on different slices of the training data. This is called **cross-validation**, and it never touches the locked test set.
- **Simple model: big improvement.** Letting it combine factors (e.g. "business AND Vistara") cut its
  average miss from about 4,550 to about 3,280 rupees, and R² went from 91 % to 95 %.
- **Random Forest: almost no change** (1,120 to 1,117 rupees). Its default settings were already about as good as they get.

## Step 7: What actually drives the price
We tested which information matters most by **scrambling one column at a time** and seeing how much worse the guesses
got. The more the guesses suffer, the more important that column is.
1. **Class** (business or economy), by far the biggest
2. **Trip duration, airline and route** (where from and where to)
3. **How many days before the flight you book**
4. **Weekday:** almost no effect

## Conclusion
**We recommend the Random Forest.**
- Its average error is about **3 times smaller** than the simple model's (1,117 vs. 3,277 rupees after tuning).
- It is accurate even *within* the same class: about **640 rupees off on an economy ticket**, roughly 10 % of the fare.
- The simple model is only better if you need something extremely easy to explain, or something that trains in under a second.

**Limitations:** the data only covers Indian domestic flights in Feb-Mar 2022. For other countries or years the model
would need to be retrained on new data.

---

## The final step: making everything ready to hand in
After all the analysis was done, the last step was to **package the work into the two files the lab asks for**:

1. **Re-running the whole notebook from scratch.** We changed the notebook a few times along the way (we improved the
   tuning of the simple model and fixed some chart details). So the whole thing is run once more from the first line to the last.
   This guarantees that every number, table and chart in the notebook comes from the same final version of the code, with
   nothing stale left over. It takes about 5 minutes, mostly spent testing the many Random Forest settings.
   During this run the notebook also saves:
   - all charts as pictures in the `figures/` folder
   - all final numbers in `results/metrics.json`
2. **Creating the PDF report automatically from those saved results.** The report is generated by a small script that
   reads the numbers straight from the results file and inserts the charts. Nobody types the numbers in by hand, so the
   report and the notebook **cannot disagree**. The report discusses the results only, without code, and answers the four
   questions the lab asks:
   - compare the two models using MAE, RMSE and R²
   - which features are the most influential
   - strengths and weaknesses of each model
   - which model we recommend, and why

---

## Files in the project folder
| File / folder | What it is |
|---|---|
| `Lab1_AirTravel_Regression.ipynb` | **Hand in.** The notebook with all code, charts and explanations |
| `Lab1_AirTravel_Report.pdf` | **Hand in.** The written report (no code) |
| `Project_Explained.md` | This file: a plain-language summary for you |
| `data/` | The unpacked datasets (we used `dataset5-AirTravel`) |
| `figures/` | The charts, saved as pictures |
| `results/metrics.json` | All final numbers, used to build the report |
| `.venv/` | The Python setup (pandas, scikit-learn and so on); you don't need to touch it |
