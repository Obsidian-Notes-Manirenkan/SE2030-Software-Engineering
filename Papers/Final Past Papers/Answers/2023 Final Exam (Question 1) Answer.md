## 1. Question summary

Analyze the partial requirements of the **Full Bright Education (FBE)** enrollment process and draw a **sequence diagram** (25 marks), using suitable **boundary, control and entity classes**.

The process has four parts:

1. Search for universities and enrollment requirements.
2. Register (**already modeled**, so it is shown as a `ref`).
3. Book a discussion and get a counselor assigned.
4. Request an offer (offer verification is **already modeled**, so it is also a `ref`), which is validated by the third-party IMMI portal.

---

## 2. Lifelines

| Lifeline | Type | Source in the scenario |
|---|---|---|
| Student | Actor | "Any student can search..." |
| `:MainUI` | Boundary | Displays results, takes booking and offer requests |
| `:LoginUI` | Boundary | Used for registration (already modeled) |
| `:FBEController` | Control | "The FBE system", which coordinates everything |
| `:Programs_Core` | Entity | Stores search details, discussions and candidate status |
| `:CounselorDB` | Entity | Counselor details and expertise areas |
| `:IMMIController` | Control | The third-party portal's controller (IMMI.gov.uk) |
| `:IMMIDB` | Entity | IMMI database used for validation |

---

## 3. Sequence diagram

```mermaid
sequenceDiagram
    actor Student
    participant MainUI as :MainUI «boundary»
    participant LoginUI as :LoginUI «boundary»
    participant FBE as :FBEController «control»
    participant PC as :Programs_Core «entity»
    participant CD as :CounselorDB «entity»
    participant IMMI as :IMMIController «control»
    participant IDB as :IMMIDB «entity»

    Note over Student,IDB: 1. Search universities
    Student->>MainUI: searchPrograms(age, qualification, areaOfStudy, studyLevel)
    MainUI->>FBE: searchPrograms(details)
    FBE->>PC: getUniversities(details)
    PC-->>FBE: universities and requirements
    FBE-->>MainUI: showResults(universities)
    MainUI-->>Student: displayResults()
    FBE->>PC: storeSearchDetails(details)
    PC-->>FBE: stored
    FBE->>FBE: analyzeTrends()
    FBE-->>MainUI: advertiseTrends()

    Note over Student,LoginUI: 2. ref: Register (already modeled): passport no. and full name via LoginUI, returns UFE number

    Note over Student,IDB: 3. Book a discussion
    Student->>MainUI: bookDiscussion(UFE, areaOfStudy, university)
    MainUI->>FBE: bookDiscussion(details)
    FBE->>PC: storeDiscussion(details)
    PC-->>FBE: stored
    FBE-->>MainUI: discussionNotification()
    FBE->>CD: getCounselors(university, expertiseArea)
    CD-->>FBE: counselorList
    FBE->>FBE: assignCounselorRandomly()
    FBE-->>MainUI: notifyCounselor(counselor)
    MainUI-->>Student: displayCounselor()

    Note over Student,IDB: 4. Request the offer
    Student->>MainUI: requestOffer(UFE, details)
    MainUI->>FBE: requestOffer(details)
    Note over FBE,PC: ref: Verify offer details via Programs_Core (already modeled)
    FBE->>IMMI: sendDetails(details)
    IMMI->>IDB: validate(details)
    IDB-->>IMMI: validationResult

    par Notify the student
        alt offer successful
            IMMI-->>FBE: successNotification()
            FBE-->>MainUI: showSuccess()
            IMMI-->>MainUI: sendOffer(offer)
        else offer unsuccessful
            IMMI-->>FBE: regretNotification()
            FBE-->>MainUI: showRegret()
        end
    and Update candidate status
        IMMI->>PC: updateStatus(candidateStatus)
        PC-->>IMMI: updated
    end
    MainUI-->>Student: displayResult()
```

---

## 4. Points that earn marks

- **Boundary, control and entity classes** are used, as the note in the question allows.
- **`ref` fragments** stand in for registration and offer verification, since the paper says they are "already modeled for reuse". Do not redraw them. In Mermaid they appear as notes, but on paper draw a proper `ref` box.
- **`alt` fragment** for the successful vs unsuccessful offer, since the IMMI controller "may send" a success notification or a regret notification.
- **`par` fragment** because the IMMI controller updates the status "simultaneously" with notifying the student.
- **Self-messages** for `analyzeTrends()` and `assignCounselorRandomly()`, since these are processing steps inside the controller.
- **The offer goes directly from IMMI to MainUI**, while the notification goes through the FBE system. The paper words the two paths differently, so draw them differently.
- **Message order** follows the text: search, register, book, then offer.
