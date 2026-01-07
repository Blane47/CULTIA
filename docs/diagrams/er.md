# CULTIA – Entity Relationship Diagram

```mermaid
erDiagram
    USER {
        string userId PK
        string fullName
        string username
        string email
        string passwordHash
        string role "USER|ADMIN|CURATOR"
        string preferredLanguageCode FK
        datetime createdAt
        datetime lastLoginAt
        boolean isActive
    }

    REGION {
        string regionId PK
        string name "e.g., Centre, Littoral, North West"
    }

    TRIBE {
        string tribeId PK
        string name "e.g., Bamileke, Bassa, Fulani"
        string regionId FK
        string description
    }

    LANGUAGE {
        string languageCode PK
        string name "English, French, Ewondo, Fulfulde, Bassa..."
        boolean isLocalDialect
    }

    CONTENT_ITEM {
        string contentId PK
        string type "ARTICLE|STORY|TRADITION|HISTORY"
        string title
        string summary
        string bodyText
        string languageCode FK
        string tribeId FK
        string regionId FK
        string status "DRAFT|IN_REVIEW|PUBLISHED|REJECTED|ARCHIVED"
        string createdBy FK
        string approvedBy FK
        datetime createdAt
        datetime updatedAt
        datetime publishedAt
    }

    SOURCE_REFERENCE {
        string sourceId PK
        string contentId FK
        string sourceType "BOOK|ELDER_INTERVIEW|ARCHIVE|WEBSITE"
        string citationText
        string url
        datetime capturedAt
    }

    TAG {
        string tagId PK
        string name "e.g., Folklore, Values, Proverbs"
    }

    CONTENT_TAG {
        string contentId FK
        string tagId FK
    }

    MEDIA_ASSET {
        string assetId PK
        string contentId FK
        string mediaType "IMAGE|AUDIO|VIDEO"
        string storageUrl
        string caption
        string languageCode FK
        datetime uploadedAt
    }

    QUIZ {
        string quizId PK
        string title
        string description
        string languageCode FK
        string tribeId FK
        string regionId FK
        string difficulty "EASY|MEDIUM|HARD"
        string status "DRAFT|PUBLISHED|ARCHIVED"
        string createdBy FK
        datetime createdAt
    }

    QUIZ_QUESTION {
        string questionId PK
        string quizId FK
        string questionText
        int points
        int orderIndex
    }

    ANSWER_OPTION {
        string optionId PK
        string questionId FK
        string optionText
        boolean isCorrect
    }

    USER_QUIZ_ATTEMPT {
        string attemptId PK
        string userId FK
        string quizId FK
        int score
        int totalPoints
        datetime startedAt
        datetime completedAt
    }

    USER_ANSWER {
        string userAnswerId PK
        string attemptId FK
        string questionId FK
        string selectedOptionId FK
        boolean isCorrect
        int pointsAwarded
    }

    ACHIEVEMENT {
        string achievementId PK
        string title
        string description
        string ruleKey "e.g., QUIZ_STREAK_3"
        int thresholdValue
    }

    USER_ACHIEVEMENT {
        string userId FK
        string achievementId FK
        datetime earnedAt
    }

    LEARNING_PROGRESS {
        string progressId PK
        string userId FK
        string contentId FK
        int percentComplete
        datetime lastViewedAt
        boolean isBookmarked
    }

    CHAT_SESSION {
        string sessionId PK
        string userId FK
        datetime startedAt
        datetime endedAt
    }

    CHAT_MESSAGE {
        string messageId PK
        string sessionId FK
        string sender "USER|BOT"
        string messageText
        datetime sentAt
        string intentDetected
        float confidence
    }

    CONTENT_REVIEW {
        string reviewId PK
        string contentId FK
        string reviewerId FK
        string decision "APPROVE|REJECT|REQUEST_CHANGES"
        string comments
        datetime reviewedAt
    }

    %% Relationships
    REGION ||--o{ TRIBE : "contains"
    REGION ||--o{ CONTENT_ITEM : "context"
    TRIBE  ||--o{ CONTENT_ITEM : "belongs_to"
    LANGUAGE ||--o{ CONTENT_ITEM : "written_in"

    USER ||--o{ CONTENT_ITEM : "creates"
    USER ||--o{ CONTENT_REVIEW : "reviews"
    USER ||--o{ LEARNING_PROGRESS : "tracks"
    USER ||--o{ USER_QUIZ_ATTEMPT : "takes"
    USER ||--o{ CHAT_SESSION : "starts"
    USER ||--o{ USER_ACHIEVEMENT : "earns"

    CONTENT_ITEM ||--o{ SOURCE_REFERENCE : "cited_by"
    CONTENT_ITEM ||--o{ MEDIA_ASSET : "has"
    CONTENT_ITEM ||--o{ CONTENT_TAG : "tagged_with"
    TAG ||--o{ CONTENT_TAG : "applies_to"

    QUIZ ||--o{ QUIZ_QUESTION : "has"
    QUIZ_QUESTION ||--o{ ANSWER_OPTION : "offers"
    USER_QUIZ_ATTEMPT ||--o{ USER_ANSWER : "records"
    QUIZ ||--o{ USER_QUIZ_ATTEMPT : "attempted"

    CHAT_SESSION ||--o{ CHAT_MESSAGE : "contains"
    CONTENT_ITEM ||--o{ CONTENT_REVIEW : "reviewed_in"

