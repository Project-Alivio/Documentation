# Overview

The following section will discuss the various key components to Alivio's app. At a high level, we will discuss the functionality of each component, their necessity from a user perspective, and how they were built. Before reading this section, we would highly recommend reading the Technologies section to get a feel for which languages and libraries were used in creating the components.

Alivio's app serves as a pressure ulcer risk management system. It seeks to help nurses understand the incidence of pressure ulcers in the ICU on a patient-by-patient basis as well as on a population-level basis. Furthermore, the app allows nurses to understand and synthesize patient data relavent to pressure ulcers in order to better inform their decision making when tending to patients and organizing their workflow.

## Patient Card

In order to give nurses a "patient-by-patient" view, each patient is displayed as a "card". Each card can be expanded by clicking the gray arrow at the bottom-center of the card in order to view pressure ulcer related data specfic to the patient. Each patient is uniquely identified by their bed number and ward, which is displayed when the card is both expanded and closed.

When expanding the card, a nurse can see the following pieces of data, which were all identified as relavent to pressure ulcers by SAIMER:

1. Age
2. Sex
3. Weight
4. Duration of stay in the ICU
5. [Braden Scale value](https://www.ahrq.gov/patient-safety/settings/hospital/resource/pressureulcer/tool/pu7b.html#:~:text=The%20Braden%20Scale%20was%20developed,risk%20for%20pressure%20ulcer%20development.)
6. Exact date and time when the patient was last turned
7. Average turn time (i.e. on average, what is the time since the patient was last turned)
8. Notes taken by the nurse

These values can be edited by clicking the pencil icon in the top right corner of the expanded portion of the card.

The code for the patient card component can be seen in `client/patientCard.jsx`. The styling is done in `client/styles/card.css`, and [this](https://mui.com/material-ui/react-card/) MUI component. Most of the backend functionalities pertaining to rendering the cards, updating the card's values, adding a new card, etc. live in `api/index.py`.

### Patient Card Management

In order to add and delete patient cards, a nurse should first click the pencil icon at the top center of the entire screen (above the fist card displayed, next to the search bar and sorting features) to perform any additions or deletions. To add a new patient, the nurse can click the plus icon and a dialog will pop up listing all the information that will be needed to generate a new card. To delete a patient, the nurse should click the red trash can icon at the top right of the desired patient card.

The add patient functionality code can be found in `client/addPatient.jsx` and all other necessary functionalities for these two components can be found in `client/patientCard.jsx`. To create the add patient component, MUI's [Dialog](https://mui.com/material-ui/react-dialog/) and [DateTimePicker](https://mui.com/x/react-date-pickers/date-time-picker/) components were used.

### Clock

One of the key features on our patient card is the clock mounted to the right of the patient's bed and ward. Here are they key functionalities of clock:

- At the center of the ring that represents the clock, the time since the patient was last turned is displayed.
- The ring "fills up" with solid color to denote how close the time to turn the patient again is.
    - When the ring is less than 1/3 full it is green, between 1/3 and 2/3 full it is orange, and more than 2/3 full it is red.
    - The ring fills up at a rate proportional to the patient's Braden Scale value. Based on numbers given to us by SAIMER, a patient at high risk for pressure ulcers needs to be turned every two hours, whereas for medium risk the time is three hours and low risk is two hours. Risk categorization is determined by the creators of the Braden Scale; further information can be found in the Braden Scale link in the card section above.
- To reset the clock back to 0h0m (i.e. a patient has been turned), a nurse should double tap inside the ring, as indicated by the text below the time.
- In the event that a nurse accidentally double taps the clock, they can reset back to the original time displayed by double tapping again. There is a thirty second window built in to perform this reset.

This clock is a key feature in bringing rich visuals to the app and helping nurses understand where all patients stand at any given moment regarding their turn timing. All front end work for the clock can be found in `client/clock.jsx` and `client/styles/clock.css`; since the clock is a child component of the patient card, state is lifted upwards to the patient card component and therefore other clock functionalities can also be found in `client/patientCard.jsx`. All backend work currently lives in `api/clock.py`.

The ring component was built using a React package called `react-circular-progressbar`.

### Ulcer Map

When the patient card is expanded, a diagram of two human bodies is displayed below all the data by default. These two diagrams serve as the Ulcer Map component, which is a way for nurses to mark where the given patient has ulcers on their body. In order to mark the ulcers, the nurse would have to click the pencil icon to trigger edit mode and then tap the area of the body where the ulcer is. Then, a red circle will appear there to denote it. If a nurse would like to remove that circle, they can tap the circle and it will disappear.

The Ulcer Map is a child component of the patient card and all of its front end code can be found in `client/ulcerMap.jsx` (as well as `client/patientCard.jsx`). The design of the component was inspired by [this](https://stackoverflow.com/questions/63851789/add-a-circle-when-clicking-on-an-img-in-react) StackOverflow post and corresponding CodeSandbox.

> [!Info]
> Using a CodeSandbox for frontend Javascript work can be really helpful. It provides a way for you to write some code and see how it works in an isolated environment without needing any IDE set up on your end.

### Trends

When the patient card is expanded, there are two icons in the top right corner of the expanded portion when in the default state: a pencil icon (edit mode) and a trend icon. When the trend icon is clicked on, the Ulcer Map changes to the Trends component. When the Trends component is displayed, the trenc icon toggles to a human body icon to toggle back to the Ulcer Map.

The Trends component displays a graph of how the patient's Braden Scale value changes over time (tracked in the unit of days). This allows for a nurse to gain a better understanding of a patient's progression regarding pressure ulcers in a visual way.

The Trends component is a child component of the patient card and all of its front end code can be found in `client/patientStats.jsx` (as well as `client/patientCard.jsx`). The React graphing library [Victory](https://formidable.com/open-source/victory/) was used to make the chart.

### Braden Scale Calculator

In order to actually calculate a patient's Braden Scale value, the Braden Scale Calculator component provides an interface to input the specific values for each category in the scale. The calculator icon, which appears next to the Braden Scale value when edit mode is on, will display the caclulator when clicked.

All front end code can be found in `client/calculator.jsx`, which serves as the base dialog pop up for the component, and `client/bradenInputs.jsx`. All backend code can be found in `api/braden.py`. In order to create this component, MUI's [Dialog](https://mui.com/material-ui/react-dialog/) and [ToggleButtonGroup](https://mui.com/material-ui/api/toggle-button-group/) components were used.

> [!Info]
> If you haven't already learned about what the Braden Scale is at this point, please take the time to do some research and understand it, as it is a key part of our app.

## View Management

In order to make the interface of all the patient cards more manageable, we provide three different ways to focus in on a subset of patients based on specific characteristics.

Code for the search bar and sorting can be found in `client/home.jsx` and code for filtering can be found in `client/filtering.jsx`.

### Search Bar

A nurse can search for specific patient cards based on the notes in their cards.

### Sorting

A nurse can sort the cards based on four different criteria:

1. Braden Scale value. This is the default way the cards are sorted, as it places higher risk patients (those with lower Braden Scale values) at the top of the list.
2. Bed. This sorts the cards in ascending order by bed id.
3. Time since last turn. This allows for nurses to see which patients were turned the longest time ago at the top.
4. Time till next turn. This sorts the cards as though the patients were in a queue, as those patients who are to be turned soon will be at the top.

This feature used MUI's [Select](https://mui.com/material-ui/react-select/) component.

### Filtering

A nurse can click the filter icon located in the top right corner of the screen to open the filtering component. This component allows them to filter patients out based on the criteria of Braden Scale value, time since last turn, and stay duration.

1. Braden Scale: filter buckets form the three pressure ulcer risk categories (high, medium, low).
2. Last Turn Time: filter buckets fall in increments of one hour for a total of four buckets.
3. Stay Duration: filter buckets form the four quartiles of the distribution of stay duration amongst all patients.

This component can allow nurses to look at a subset of patients based on a variety of different criteria combinations. It uses MUI's [Checkbox](https://mui.com/material-ui/react-checkbox/) and [Drawer](https://mui.com/material-ui/react-drawer/) components.

## Statistics

This feature allows nurses to analyze pressure ulcer activity from an ICU-level perspective as a whole. It seeks to provide them with an analysis of the trends occuring in the ICU in order to understand how they might adjust their caretaking based on these numbers. The Statistics feautre consists of three main parts:

1. Median Statistics: reports the median Braden Scale, Stay Duration, and Time Since Last Turned values across the whole ICU.
2. Archive Table: when a patient is deleted, their row from the patient table in the database is moved to a separate "archive" table. This way, we can analyze trends amongst those who have completed their ICU stay to understand what their stay was like.
3. Braden Bucket Statistics: buckets each archive patient into a Braden risk category and examines the trends in each bucket. For example, for the patients in the high risk bucket, this feature reports what the average age, weight, stay duration, etc. is.

Code for this component can be found in `client/stats.jsx`, `client/bradenBucketStats.jsx`, and `api/stats.py`. It was created using MUI's [Dialog](https://mui.com/material-ui/react-dialog/) and [Accordian](https://mui.com/material-ui/react-accordion/) components.

## Feedback Form

A nurse can click the blue feedback icon on the bottom right corner of the screen if they would like to provide us feedback on the app. When they submit the feedback, it goes straight to our database with a timestamp marking when it was submitted.

Code for this component can be found in `client/feedbackForm.jsx`. Like many of our other components, this one uses MUI's Dialog component.

## Login and Logout

As a method of authentication, we ensure that nurses have the ability to login an logout of the app. Before reading about this component, please make sure you understand how token authentication works from the Architecture section.

If the current user is not in possession of a valid token, the login screen with a username and password prompt will appear. The API endpoint servicing logins will first ensure that the credentials entered are correct based on login info stored in the database. Once logged in, the user will acquire a valid token, and can logout using the button at the top left corner of the screen. Logging out will clear the current token.

You can find the code for the login component in `client/login.jsx` and `api/login.py`.