# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Anna Karlova

```
Answer 1:

Stage 1: Creating a Population of Event Attendees

Description: The code events = ['wedding'] * 200 + ['brunch'] * 800 generates a population of 1,000 individuals, with 200 attending weddings and 800 attending brunches. This sets up the foundation for simulating infection spread and contact tracing.

Sampling Frame: The 1,000 people, divided between weddings and brunches, form the initial sample. This setup reflects real-world scenarios where weddings are larger gatherings, and brunches are more numerous but smaller in scale.

Purpose: This stage establishes a realistic environment where event size influences the outcomes of infection spread and contact tracing effectiveness.

Stage 2: Random Selection of Infected Individuals (Simple Random Sampling)

Code: infected_indices = np.random.choice(ppl.index, size=int(len(ppl) * ATTACK_RATE), replace=False)

Sampling Procedure: Simple random sampling is used to select 10% of the population (100 individuals) to be infected, ensuring that all individuals have an equal chance of being selected (ATTACK_RATE = 0.10).

Sample Size: 100 infected individuals, representing 10% of the population.

Underlying Distribution: Uniform random distribution for selecting infected individuals.

Purpose: This stage ensures that the infection is distributed evenly across both event types, providing a balanced starting point for the simulation before any biases related to tracing are introduced.

Stage 3: Primary Contact Tracing (Random)

Code: ppl.loc[ppl['infected'], 'traced'] = np.random.rand(sum(ppl['infected'])) < TRACE_SUCCESS

Sampling Procedure: To determine whether an infected individual will be traced, a 20% chance (TRACE_SUCCESS = 0.20) is applied. Random numbers are generated for each infected person, and if the number is less than 0.20, they are traced.

Sample Size: 20 individuals out of the 100 infected are traced.

Purpose: This step simulates real-world contact tracing limitations, where only a subset of infected individuals can be traced due to factors like resource constraints and incomplete information. As a result, this sample is biased and incomplete.

Stage 4: Secondary Contact Tracing (Cluster)

Code: event_trace_counts = ppl[ppl['traced'] == True]['event'].value_counts()

Sampling Frame: Events with at least two traced individuals (SECONDARY_TRACE_THRESHOLD = 2) are selected for secondary contact tracing.

Sampling Procedure: Once an event reaches the threshold of two traced individuals, all infected individuals at that event are traced, creating clusters of infections.

Purpose: This step introduces a bias toward larger, more easily traceable events, such as weddings, which often have structured guest lists. This biases the results, overrepresenting high-attendance events like weddings in the traced data.


Answer 2: 

The code does not reproduce the graphs from the original blog post.


Answer 3: 

When running the code multiple times with a reduced number of repetitions (1000), we can notice slightly more variability in the graphs. 
This increased variability is expected with fewer repetitions since each run's random sampling significantly impacts the results, making the output graphs less reliable. In contrast, a higher number of repetitions (like the original 50,000) provides more stable results, enhancing the reproducibility of observed proportions.


Answer 4: 

To make the code reproducible, I set the random seed to a fixed value with np.random.seed(42). This ensures that NumPy's random functions produce the same sequence of random numbers each time the code is run, making the results consistent across runs. Without a fixed seed, each run would yield different selections of infected individuals and traced cases, leading to varying outputs.


```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
