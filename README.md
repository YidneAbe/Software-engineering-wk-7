# Software-engineeringPart 1: Theoretical Understanding (30%)
1. Short Answer Questions
Q1: Define algorithmic bias and provide two examples of how it manifests in AI systems.
Algorithmic bias refers to systematic and repeatable errors in a computer system that create unfair outcomes, such as privileging one arbitrary group of users over others. It arises when an algorithm produces results that are systematically prejudiced due to erroneous assumptions in the machine learning process.
Examples:
1.	Hiring Algorithms: Amazon's recruiting tool was trained on resumes submitted over a 10-year period, which were predominantly from male applicants. The model learned to penalize resumes that included words like "women's" (as in "women's chess club") and downgraded graduates from all-women's colleges, thereby discriminating against female candidates.
2.	Predictive Policing: Predictive policing systems trained on historical crime data can reinforce existing societal biases. If a police force has historically patrolled minority neighborhoods more heavily, the data will show more crimes reported in those areas. The algorithm then recommends deploying even more resources to these neighborhoods, creating a feedback loop of over-policing while under-policing other areas.
Q2: Explain the difference between transparency and explainability in AI. Why are both important?
•	Transparency is about the openness of the AI system's design, data, and processes. It answers the question, "Can we see how this system was built?" This includes documenting the data sources, model architecture, and training methodologies. A transparent process allows for external auditing.
•	Explainability (or Interpretability) is about the ability to understand and articulate the reasoning behind a specific decision or output of an AI system. It answers the question, "Can we understand why it made this specific decision for this specific case?"
Both are important because they are foundational to trust and accountability. Transparency builds trust with regulators and the public by demystifying the AI creation process. Explainability builds trust with end-users (e.g., a loan applicant denied by an AI) and developers, allowing them to debug models, ensure they are working as intended, and challenge incorrect or biased decisions.
Q3: How does GDPR (General Data Protection Regulation) impact AI development in the EU?
The GDPR imposes several legal requirements that directly shape AI development:
1.	Lawfulness, Fairness, and Transparency (Article 5): AI systems must be designed to process data lawfully and transparently. This mandates explainability for automated decisions.
2.	Right to Explanation (Article 22): Individuals have the right not to be subject to a decision based solely on automated processing, including profiling, which produces legal or similarly significant effects. They can obtain human intervention and contest the decision.
3.	Data Minimization and Purpose Limitation: AI models must be built using only data that is necessary for a specific purpose, discouraging the "collect everything" approach that can lead to privacy violations and embedded biases.
4.	Data Subject Rights: Rights like the "Right to Erasure" (to be forgotten) mean that AI systems must be designed in a way that allows for specific personal data to be removed from models, which is a technically challenging requirement.
2. Ethical Principles Matching
•	(B) Non-maleficence - Ensuring AI does not harm individuals or society.
•	(C) Autonomy - Respecting users’ right to control their data and decisions.
•	(D) Sustainability - Designing AI to be environmentally friendly.
•	(A) Justice - Fair distribution of AI benefits and risks.
________________________________________
Part 2: Case Study Analysis (40%)
Case 1: Biased Hiring Tool
•	Source of Bias: The primary source of bias was the historical training data. The model was trained on a decade's worth of resumes from a male-dominated tech industry. It learned to associate male candidates with being successful hires, thus inferring that being male was a positive predictor and penalizing patterns associated with female candidates.
•	Proposed Fixes:
1.	Debias the Training Data: Use techniques like re-sampling to create a more balanced dataset or re-weighting the importance of examples from underrepresented groups (female candidates) during training.
2.	Remove Proxy Variables: Actively identify and remove features that serve as proxies for gender. This includes not just explicit fields like "gender," but also words, phrases, club memberships, and university names that the model might be using to infer gender.
3.	Implement a "Blind" Application Review: Use an AI tool not to score candidates, but to anonymize applications by redacting gender-revealing information, photos, and names before human review. The AI's role shifts from decision-making to bias-filtering.
•	Fairness Metrics:
o	Demographic Parity: Check if the rate of candidates selected is similar across gender groups.
o	Equality of Opportunity: Check if the True Positive Rate (the rate at which qualified candidates are correctly recommended) is similar for both men and women.
o	Predictive Rate Parity: Ensure that the probability of a candidate being qualified, given they were recommended, is the same for both groups.
Case 2: Facial Recognition in Policing
•	Ethical Risks:
o	Wrongful Arrests and Convictions: The higher misidentification rate for minorities can lead to innocent people being detained, arrested, or even convicted, eroding trust in the justice system.
o	Mass Surveillance and Chilling Effects: Widespread use can lead to a surveillance state, discouraging freedom of assembly and expression in public spaces.
o	Reinforcement of Systemic Bias: The technology could be deployed disproportionately in minority neighborhoods, generating more data and creating a feedback loop that justifies further surveillance.
o	Lack of Due Process and Explainability: It can be difficult for a defendant to challenge an AI's "match," especially if the algorithm is a proprietary "black box."
•	Policies for Responsible Deployment:
1.	Mandatory Third-Party Audits: Require independent, transparent audits for accuracy and bias (e.g., using NIST standards) before deployment.
2.	A Moratorium on "Real-Time" Use: Ban the use of live facial recognition on public video feeds to identify individuals in real-time, as the risks of error are too high. Restrict its use to post-incident investigation.
3.	Strict Legislative Oversight: Pass laws requiring warrants for most uses of facial recognition, similar to rules for search and seizure.
4.	Public Transparency and Consent: Mandate public reporting on where and how the technology is used, its error rates, and its demographic impact. Use in public spaces should require community consultation.
________________________________________
Part 3: Practical Audit (25%)
Deliverable: The code for this audit is provided in the accompanying Jupyter Notebook file (AI_Ethics_Audit.ipynb), which will be shared via GitHub. Below is the summary report.
Report: Audit of the COMPAS Dataset for Racial Bias
Objective: This audit analyzes the COMPAS (Correctional Offender Management Profiling for Alternative Sanctions) dataset to determine if the risk assessment algorithm exhibits racial bias, particularly against African-American defendants.
Methodology: Using IBM's AI Fairness 360 (AIF360) toolkit in Python, we loaded the COMPAS dataset and preprocessed it to focus on the recidivism prediction within two years. We defined African-American defendants as the privileged group and Caucasian defendants as the unprivileged group for the bias analysis. Key metrics calculated included:
•	Disparate Impact: The ratio of favorable outcomes (being labeled low-risk) for the unprivileged vs. privileged group.
•	Average Odds Difference: The average of the difference in False Positive Rates (FPR) and True Positive Rates (TPR) between the two groups.
•	Statistical Parity Difference: The difference in the probability of being assigned a favorable outcome.
Findings:
The analysis revealed significant racial disparities, corroborating ProPublica's investigation. The key findings were:
1.	Higher False Positive Rate for African-Americans: African-American defendants were nearly twice as likely as white defendants to be falsely labeled as high-risk (i.e., predicted to re-offend but did not). This is a critical error, as it can lead to harsher sentencing and prolonged detention for innocent people.
2.	Lower False Negative Rate for African-Americans: Conversely, white defendants who did re-offend were more likely to be incorrectly labeled as low-risk compared to African-American re-offenders.
3.	Disparate Impact < 1: The calculated Disparate Impact was significantly below the 0.8 threshold (often used to indicate a substantial bias), confirming that the model's outcomes were not equitable across racial groups.
Remediation Steps:
To mitigate this bias, several techniques can be applied:
1.	Pre-processing: Use reweighing (a technique in AIF360) to assign weights to training instances to minimize bias before the model is trained.
2.	In-processing: Employ fairness-aware algorithms during model training that explicitly include a fairness constraint in their optimization objective.
3.	Post-processing: Adjust the output thresholds of the model differently for different groups to equalize error rates (e.g., equalize FPR). This is a direct but often controversial fix.
4.	Re-evaluation of Features: Critically audit the input features used by COMPAS (e.g., criminal history, social factors) to identify and remove proxies for race.
Conclusion: The COMPAS algorithm, as reflected in this dataset, demonstrates clear and measurable racial bias. This audit underscores the critical necessity of conducting rigorous fairness audits as a standard practice before deploying any AI system in high-stakes domains like criminal justice.
________________________________________
Part 4: Ethical Reflection (5%)
Prompt: Reflect on a personal project (past or future). How will you ensure it adheres to ethical AI principles?
For a future project, such as developing a personalized learning recommendation system for an online academy, ensuring ethical adherence would be a core priority from the outset. I would integrate ethics using a multi-stage approach guided by frameworks like the EU Ethics Guidelines for Trustworthy AI.
1.	Ethical Scoping & Data Collection: Before writing any code, I would conduct a "pre-mortem" risk assessment. What are the potential harms? Could the system unfairly recommend STEM courses to male students and humanities to female students based on biased data? I would meticulously document the data sources, ensuring they are diverse and representative. I would also implement strict data anonymization and adhere to GDPR/privacy principles from day one.
2.	Fairness-by-Design Development: During development, I would use tools like AIF360 to continuously audit the model for bias across demographics like gender, race, and socioeconomic status (using proxies like location, if necessary and privacy-compliant). I would prioritize explainability by building a feature that allows a student to ask, "Why was this course recommended to me?" The answer wouldn't be "the algorithm said so," but would point to concrete factors like "because you performed well in a prerequisite course X and students like you enjoyed Y."
3.	Transparent Deployment and Monitoring: Upon deployment, I would be transparent with users about how the system works and what data it uses. I would provide clear opt-out options, respecting user autonomy. Crucially, I would establish a continuous monitoring pipeline to detect performance drifts or emerging biases, ensuring the system remains just and beneficent long after its initial launch. This proactive and integrated approach moves ethics from a mere checklist to a foundational component of the AI system's lifecycle.

