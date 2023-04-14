---
title: "An excuse to use GPT-3: generating patient histories and lesson plans"
date: 2023-01-31
---

I wanted to see how well GPT-3 performed without any tuning for specialised areas of knowledge. In particular, I wanted to see how useful GPT-3 capabilities could be for two applications in the public sector. I have zero medical or teaching knowledge, but I do have some anonymous doctor/teacher friends.

I investigated whether `text-davinci-003`, the most capable GPT-3 model available through the OpenAI API, could be useful for; 



1. Creating synthetic patient histories, which could be useful for medical data science and medical training itself
2. Summarising patient histories, which could be useful in clinical settings to reduce clerical sifting time and human error.
3. Creating lesson plans for primary school children, which could reduce the planning burden for teachers.

**What I did**


<table>
  <tr>
   <td rowspan="2" colspan="2" >
   </td>
   <td colspan="3" >Task for GPT-3
   </td>
  </tr>
  <tr>
   <td>Synthetic patient history generation
   </td>
   <td>Summarising a long patient history
   </td>
   <td>Creating lesson plans
   </td>
  </tr>
  <tr>
   <td rowspan="3" >Participants
   </td>
   <td>me
   </td>
   <td>✔
   </td>
   <td>✔
   </td>
   <td>✔
   </td>
  </tr>
  <tr>
   <td>Two medical professionals
   </td>
   <td>✔
   </td>
   <td>✔
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>One teacher
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>✔
   </td>
  </tr>
</table>


Using the ‘completion’ call to the Davinci model, I prompted GPT-3 with four variations of the same input and asked it to generate a patient history. The input varied in how I phrased the request, but always gave the model the instruction to make up a patient history; that the patient was female, 87 years old, and currently presenting back pain and dizziness; the instruction to include full medical history, including allergies, incidents and previous visits; the instruction to make the patient history long and detailed. 

Prompts used to generate histories 2-4 also requested the output include changes to conditions. I did not supply names or any other medical details. Prompt 1 was written with a token limit of 500 and a temperature of 0. Prompts 2, 3 and 4 were written with a token limit of 2048 and a temperature of 0.8, as OpenAI recommends a [higher sampling temperature for more creative application](https://platform.openai.com/docs/api-reference/completions)s.

I also prompted the model to help me create a series of long and convoluted notes on a patient. This human-in-the-loop (me) approach consisted of me writing a prompt to create patient notes, passing it to GPT-3, then using the output to write a new prompt. I then concatenated all the outputs together into one long patient history, as might be compiled by multiple busy nurses over years of visits. I also used the text-davinci-edit-001 engine to make deliberate mistakes and edits in the final stitched-together piece.

Finally, I fed this long patient history back to GPT-3 and asked it to summarise the history, given the scenario that the “current primary symptoms [for the patient were] shortness of breath and wheezing cough.”

I then put the outputs to two doctor friends for their opinions (as well as judging it myself). For each of the four patient histories, I asked them to note any feedback or thoughts they had on the patient history generated, in particular whether they thought they were realistic. I also asked them to summarise the long patient history, for comparison to GPT-3’s summary, and then for their opinions on how GPT-3’s summary fared.

Concurrently, I gave GPT-3 a prompt to create four lesson plans, based on learning objectives provided by a primary school teacher (who teaches Year 3 in England). The temperature of the model was set at 0.8 every time. Each lesson plan was for a different lesson objective, but the prompts always contained the following information:



* The instruction to create a lesson plan.
* The specification of lesson length (one hour) and subject (maths, English or science).
* The specification that the lesson plan should be targeted for seven-year-old-children.
* The specification that the lesson plan should account for differences in ability. 

As in the case of the patient's histories, I then put the outputs to my teacher friend for their opinion, as well as judging myself. 

Full prompt material is available [here on my project repo](https://github.com/myndrws/exp_gpt).

**Interesting results**

_Four patient histories_

The first prompt used the most imprecise language; the second is still imprecise but removes some of the most unnecessary wording. Whilst both doctors agreed that the generation seemed plausibly human-level, one made clear it was not at good medical-professional level. This is in spite of the prompt containing instructions to mimic the style of an NHS Doctor. The second prompt actually seemed to perform less well, as the generation didn’t provide an overview.

The third prompt used to generate a synthetic patient history reminded GPT-3 twice about the length of the patient history needing to be long; this successfully produced a longer response than the previous two prompts. 

For the fourth prompt I kept the in the repeated instruction to make the patient history long, and  tried a format which could be easily standardised and augmented by selecting from random characteristics with python. This could mean that to generate comprehensive synthetic medical histories, we only need to randomly select sex, age and from a list of possible presenting symptom words. 

The fourth prompt resulted in a response that actually followed the directions given more closely (i.e. “Make the patient history convoluted and detailed”). This is potentially because the prompt was made more concise by removing supplementary language around the prompt description of the patient (i.e. altering “This is for a patient that is female and 87 years old and is now complaining of lower back pain and dizziness. This patient history needs to include full medical history including allergies and incidents and previous visits. It should also include any changes to conditions over the patient's lifetime.” to instead read “Use patient details: sex: female, age: 87, current symptoms: back pain, dizziness. Include full medical history including allergies, incidents, previous visits, changes to conditions.”). Evidence for this is the response including details on the patient’s hobbies. This format also seemed to encourage GPT-3 to respond with much more specialist medical terminology - for instance, referring to knee effusion and sulfonamides.

The doctors I asked about the outputs from the four prompts noticed the difference between the first three and the last prompt, both concluding that the fourth prompt resulted in the most doctor-like patient history write up.

Medically speaking, it was clear from the feedback from medical professionals that there were multiple mistakes and within-generation inconsistencies on closer examination. There were ‘subtle inconsistencies’ that sounded true; for instance, it was pointed out that as the NHS was founded in 1948, it was impossible for the currently 87-year-old patient to have come to the NHS when she was eight years old.

Also evident from the feedback were that some conditions mentioned in the generation fit the pattern of what would be expected, whereas some did not. This brought ‘plausibility vs impossibility’ sharply into focus. For instance, in generations that suggested the patient had osteoporosis, the doctors tended to agree this was quite realistic. All four fabrications cited that the patient had asthma, suggesting that the model has seen more data relating to this particular ailment. This is backed up by the accuracy of descriptions of asthma - for instance, the generation from prompt two suggested that the asthma had been managed into adulthood, which one doctor pointed out seemed quite accurate because asthma does often go away in adulthood. This said, another two describe late onset of asthma, which as pointed out by the doctors is not impossible but is rare. 

Similarly, two of the four histories detailed allergies to penicillin and, to quote a doctor,  “'sulfa' drugs (sulphonamides fall into this category)”. They went on to say: “Of things to be allergic to, these are certainly near the top of this list. Rates of penicillin and sulfa drug allergy are in the order of 1-10% (big range as most people who think they're allergic actually aren't). The probabilities of each aren't independent of each other as people who are allergic to one are more likely to be allergic to the other, but does still seem a bit of a coincidence. I'd be interested to know what the spread of this was if you ran the model more times.”

Across the four prompts, even though there was nothing in the prompt to suggest that the patient would be married, the response always contained a reference to the patient as ‘Mrs’. This indicates possible age-related biases, at least in the context of people who have their sex defined as ‘female’ on medical records. Biases like this one are pertinent to consider if using the model to build banks of synthetic data; if the prompt combination of `older` + `female` always results in the implicit assumption that a patient is or has been married, this of course is exclusionary and misses generating diverse synthetic patient histories.

_Human-in-the-loop synthetic patient history generation_

GPT-3 actually tried to correct me when I ran the long patient history through the editing prompt "add in some mistakes and irrelevant details". It tried to make the patient’s age the same throughout (modifying any mention of age to 61), when the point was to mimic notes taken over years by different people, necessitating that the patient would be different ages. Accordingly, I changed these back.

Feedback from the doctors included that it felt like the model had generated histories for four different patients; this is likely an artefact of the way in which the text was produced. This made it harder to summarise the history for this person. 

_Summarising patient histories_

I then put this combination of prompts and outputs all back into GPT-3 as a single prompt with the preceding instruction to “Summarise all the below content to 3 sentences given current primary symptoms are shortness of breath and wheezing cough”. It produced five sentences, but had definitely paid some attention to removing unnecessary information. For instance, it did not mention the patient’s vertigo, presumably because this was unrelated to the current symptoms. This said, it did mention allergies, and rather than reporting the outcomes of previous tests, the model simply reported that tests were recommended - this might be due to time-related confusion in the case notes (a human might deduce that later notes refer to later events, but this might be harder for the model if implicit only - it had failed to cross-reference with the changes in patient age reported at the start of each block of notes).

_Creating lesson plans_

I used initial learnings from the writing of patient histories to make a better go at the input prompts for this task; for instance, using less fluffy or filler language. Thus the only thing that changed between prompts was the specification of the subject and the objective of the lesson. 

In all of the responses, the model gave a clear ‘layout’ for the lesson plan - for instance, providing section headers and accompanying timings for each section. On two of the four tries, the model failed to make the sections add up to an hour - almost as though it was getting carried away. In two of the responses, the model provided a section on required materials.

Each generated lesson plan result had its own flair, as would be the case between teachers, even though each prompt contained basically the same instructions. For instance, one lesson plan (from prompt three) explicitly laid out how to deal with differing abilities, a core prompt requirement; two outputs mentioned ability implicitly (outputs from prompt one and three), and the others ignored this. 

There are some impressive mentions of the objective in the lesson plans. Choice quotes include:



* “Give each student a worksheet with a three digit number that they must identify and explain the place value of.”
* “Ask the students to draw pictures of different objects. Once the pictures are finished, have them write a noun phrase to describe the picture”
* “20 minutes | Presenting the concept of conjunctions and how they are used to join two ideas together in a sentence. Introduce the list of conjunctions (and, but, or, so, because, etc.) and give examples of how they are used in sentences.”
* “2. Each group will be given a set of skeletons (either from a book, poster, or 3D model) [...] asked to identify the different components of the skeleton, how it supports, moves, and protects”

That said, most of the instructions remained pretty vague - for instance, the model seems to generally use worksheets as a ‘get out of jail free’ card, specifying that they should be used but not necessarily what they’re about. 

**New prompts based on learnings + OpenAI prompt design article**

Following my exploration, I now had new hypotheses for what made a good prompt for the patient history generation task, the patient history summarisation task and the lesson plan generation task. My only artificial constraint was keeping the process zero-shot (i.e. I was not providing any examples to GPT-3).

_Patient history generation task_

Hypotheses: 



* Using the fourth patient history template as a starting point will produce the best results.
* Factual mistakes will be reduced by reducing the temperature.
* Style mistakes will be reduced by saying what not to do as well.
* Adding direction for what to do, not direction for what not to do, will produce best results.
* Length will be better specified by providing a number of sentences to generate, not just saying ‘long’ (i.e. being more precise).
* Including descriptive information about the desired context, outcome, length, format and style will produce best results.
* Separating the instructions at the beginning of the prompt and using """ to separate the instruction and context will increase chances of best results.

Where ‘best results’ is a synthetic generation that, as judged by the doctors, as closely as possible resembles what a human doctor would write.

Write a full patient history including medical history, allergies, incidences, previous visits, changes to condition. Write 30 sentences in the style of a UK NHS Doctor. Instead of writing as a list, write long detailed notes.

Patient details: “””

Female, 87 years old, currently experiencing lower back pain and vertigo. 

“””

**Key takeaways**

It’s pretty much all an exercise in prompt design. Prompt engineering could become its own field; [OpenAI have a list of tips and tricks to getter better responses](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api), some which help to explain why different prompts I tried out received better responses. I hadn’t found this article prior to trying out a lot of the prompting, but if I do it again this is where I’ll start.

Doctor-level factual accuracy is hard to achieve, at least when working with prompts written by a non-medical-professional. Experts need to be writing the prompts for and involved in the evaluation of any generation that is going to be used in a medical setting (I realise that’s fairly obvious, but I mean it on the deeper level of ‘writing like a human in a rushed medical setting’ is still not enough to inject medical viability into a model response). If not, GPT-3 tends to reflect the mistakes made by the prompt (e.g. using the non-specific colloquial ‘dizziness’ as a symptom prompt).

Further questions:



* Does repeating an instruction multiple times mean gpt-3 focuses more on it? E.g. format of ‘ask for A, then ask for B, then ask for A’ as simplest example - can change up where A is asked for, then switch A and B around. Would be good if this could be quantified.
    * How does the model interpret this prompting?
    * This could show that you don’t need to even give more examples - you can just repeat the thing you want?

You can view [all the prompts and model responses on my repo.](https://github.com/myndrws/exp_gpt) 