# Facets Validation

## Core Facets

### location
**Valid Values:** Real geographic references (city, state, country)

### timeZone
**Valid Values:** "PST (UTC-8)", "EST (UTC-5)", "IST (UTC+5:30)", "GMT (UTC+0)", "CST (UTC-6)"

### gender
**Valid Values:** "male", "female", "other", "prefer_not_to_say"

### commsPref
**Valid Values:** "video", "audio", "text", "any"

## Student Facets

### step
**Valid Values:** "1", "2 CK", "2 CS", "3"

### studyStyle
**Valid Values:** "flashcards", "questions", "videos", "discussion", "mixed"

### studyHours
**Valid Values:** "2 hours", "5-6 hours", "1-2 hours", "8 hours", "flexible"

### intent
**Valid Values:** "accountability", "guidance", "peer-learning", "motivation", "exam-prep"

### examDate
**Valid Values:** "12/15/2024", "2024-12-15", "next month", "in 3 months", "March 2025"

### ratesBand
**Valid Values:** "$", "$$", "$$$", "$$$$", "flexible", "doesn't matter"  
**My question:** Does it need to be like $$,$$$ or some real numbers like 70-90$?  
**Note:** Only required if selected mission is tutor

## Tutor Facets

### stepsTaught
**Valid Values:** "1", "2 CK", "2 CS", "3"

### specialties
**Valid Values:** "anatomy", "physiology", "biochemistry", "pathology"

### experienceYrs
**Valid Values:** "2 years", "5 years", "1-3 years", "0 years (new tutor)", "3"  
**My question:** Does the year need to be a number like 4, 3, or 3-5?

### ratesBand
**Valid Values:** "$", "$$", "$$$", "$$$$", "flexible", "doesn't matter"  
**My question:** Does it need to be like $$,$$$ or some real numbers like 70-90$?

### availabilityBands
**Valid Values:** "weekdays", "weekends", "evenings", "mornings", "flexible"

## Resident Facets

### specialty
**Valid Values:** "Internal Medicine", "Surgery", "Pediatrics", "Psychiatry", "Emergency Medicine"

### pgy
**Valid Values:** "1", "2", "3", "4", "5", "6", "7"

### mentoring
**Valid Values:** "study", "rotations", "career_planning", "research", "board_prep"

### availabilityBands
**Valid Values:** "weekdays", "weekends", "evenings", "mornings", "flexible"

## Mission Schema

### student_peer
Studen preferences for study partner
**Schema:** v1  
**Facets:** gender, location  

### tutor
Student preferences for tutor
**Schema:** v1  
**Facets:** stepsTaught, ratesBand, specialties, experienceYrs, availabilityBands  

### student
Tutor\Resident preferences for students
**Schema:** v1  
**Facets:** studyStylePreference, studentIntentPreference  

### resident
Student preferences for resident mentor
**Schema:** v1  
**Facets:** specialty, pgy, mentoring, availabilityBands  

## Example Schema

```json
{
  "missions": {
    "student_peer": { 
      "schema": "v1", 
      "overrides": { 
        "step": "1", 
        "studyStyle": "flashcards", 
        "location": "New York" 
      }, 
      "createdAt": 1730000000 
    }, 
    "tutor": { 
      "schema": "v1", 
      "overrides": { 
        "stepsTaught": "1", 
        "ratesBand": "$$", 
        "specialties": "anatomy, physiology", 
        "experienceYrs": "3-5 years" 
      }, 
      "createdAt": 1730000500 
    }, 
    "student": { 
      "schema": "v1", 
      "overrides": { 
        "studyStylePreference": "questions", 
        "studentIntentPreference": "exam-prep" 
      }, 
      "createdAt": 1730000600 
    },
    "resident": { 
      "schema": "v1", 
      "overrides": { 
        "specialty": "Internal Medicine", 
        "pgy": "3", 
        "mentoring": "study, career_planning", 
        "availabilityBands": "evenings" 
      }, 
      "createdAt": 1730000700 
    }
  } 
}
```

## Questions for Clarity

1) **Do we need the schema parameter in missions?**
   - Currently all missions use "v1" schema
   - Is this parameter necessary for the system or can it be removed?

2) **Do we need to ask for gender and location preference in every mission?**
   - Currently some missions have gender/location in facets, others don't
   - Should these be standardized across all missions?

3) **Does the rates band format need to be $$, $$$ or real numbers like 50-70$?**
   - Current format: "$", "$$", "$$$", "$$$$"
   - Alternative format: "50-70$", "100-200$", "500-1000$"
   - Real number ranges would make filtering system easier to implement

## Persona Formation

When onboarding completes, a profile string is formed from user's core + role information. Mission-specific details are NOT included in the persona.

### Core Persona Template
**Template:** "A {roles} located in {location}, in {timeZone} timezone, gender {gender}. Prefers {commsPref} communication."

### Role-Specific Additions

#### Student Persona
**Template:** "Student preparing for USMLE {step}, studies using {studyStyle} for {studyHours} per day, motivated by {intent}{examDate}."

**Example:** "A student located in New York, in EST (UTC-5) timezone, gender female. Prefers video communication. Student preparing for USMLE 1, studies using flashcards for 5-6 hours per day, motivated by exam-prep March 2025."

#### Tutor Persona
**Template:** "Tutor specializing in {specialties}, with {experienceYrs} years of experience, teaches USMLE Steps {stepsTaught}, charges {ratesBand} rate band, available during {availabilityBands}."

**Example:** "A tutor located in California, in PST (UTC-8) timezone, gender male. Prefers video communication. Tutor specializing in anatomy, physiology, with 3-5 years of experience, teaches USMLE Steps 1, charges $$ rate band, available during evenings."

#### Resident Persona
**Template:** "Resident in {specialty} at PGY {pgy}, can mentor in {mentoring}, available during {availabilityBands}."

**Example:** "A resident located in Texas, in CST (UTC-6) timezone, gender other. Prefers audio communication. Resident in Internal Medicine at PGY 3, can mentor in study, career_planning, available during evenings."

### Complete Persona Examples

**Student:** "A student located in New York, in EST (UTC-5) timezone, gender female. Prefers video communication. Student preparing for USMLE 1, studies using flashcards for 5-6 hours per day, motivated by exam-prep March 2025."

**Tutor:** "A tutor located in California, in PST (UTC-8) timezone, gender male. Prefers video communication. Tutor specializing in anatomy, physiology, with 3-5 years of experience, teaches USMLE Steps 1, charges $$ rate band, available during evenings."

**Resident:** "A resident located in Texas, in CST (UTC-6) timezone, gender other. Prefers audio communication. Resident in Internal Medicine at PGY 3, can mentor in study, career_planning, available during evenings."

## Matchmaking Profile Query Formation

The matchmaking system uses different profile query combinations based on the mission type:

### Student to Student Finding (student_peer)
**Profile Query:** Student's core + role + mission information
- **Core:** location, timeZone, gender, commsPref
- **Role:** step, studyStyle, studyHours, intent, examDate
- **Mission:** Only 2 extra facets (gender, location preferences)
- **Complete Query:** Uses all student facets + minimal mission preferences

### Student to Tutor (tutor mission)
**Profile Query:** Student's core + mission facets only
- **Core:** location, timeZone, gender, commsPref
- **Mission:** stepsTaught, ratesBand, specialties, experienceYrs, availabilityBands
- **Note:** Mission includes facts and questions regarding the tutor role
- **Complete Query:** Student core + tutor role requirements

### Student to Resident (resident mission)
**Profile Query:** Student's core + mission facets only
- **Core:** location, timeZone, gender, commsPref
- **Mission:** specialty, pgy, mentoring, availabilityBands
- **Note:** Mission facets are about the resident role
- **Complete Query:** Student core + resident role requirements

### Resident/Tutor to Student (student mission)
**Profile Query:** Resident/Tutor's core + mission facets only
- **Core:** location, timeZone, gender, commsPref
- **Mission:** Only 2 facets (studyStylePreference, studentIntentPreference)
- **Complete Query:** Resident/Tutor core + minimal student preferences

### Questions for Clarity

1) **What additional facets should be included for the student mission?**
   - Currently only has studyStylePreference and studentIntentPreference
   - What other student characteristics should tutors/residents consider when searching?

2) **Facet Values and Filtering Mechanisms for Facet-Graph Architecture Optimization**
   - **Current State:** Based on the details above, do you have any changes in the facet values that the system can accept?
   - **Critical Phase:** This facet-graph architecture phase is really crucial for final optimization
   - **Filtering Suggestions:** Do you have any suggestions for the filtering mechanisms for the new facets introduced?

3) **Filtering System Design and Hard Filters Recommendations**
   - **Multi-value Filtering:** For multi-values (e.g., specialties: "anatomy, physiology"), the idea is clear - find the seeker value in the list
   - **Rates Band Filtering:** What are your suggestions for filtering rates band? Should it be:
     - Exact match ($, $$, $$$, $$$$)?
     - Range-based filtering (if user wants $$, show $$ and below)?
     - Dollar range filtering (50-90$, 100-200$, 500-1000$)?
   - **Hard Filters for Each Mission:**
     - **student_peer:** What should be the mandatory hard filters? (step, location, timeZone, studyStyle?)
     - **tutor:** What should be the mandatory hard filters? (stepsTaught, location, timeZone, ratesBand, specialties?)
     - **resident:** What should be the mandatory hard filters? (specialty, pgy, location, timeZone?)
     - **student:** What should be the mandatory hard filters? (studyStylePreference, studentIntentPreference, location?)
   - **Filter Priority:** How should the system prioritize which filters to relax first when no matches are found?

