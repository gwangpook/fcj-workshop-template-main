---
title: "Week 11 Worklog"
date: 2026-04-17
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Design the core APIs.
* Build the database.
* Implement the content approval workflow.
* Optimize resource management.
* Finalize the technical documentation.

### Tasks to be completed this week:

| Day | Task | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| 2 | Design APIs for the Flashcard (POST/Create) and Quiz features, including input validation (maximum character limits and question format validation). | 28/06/2026 | 29/06/2026 | [Building RESTful APIs with API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started.html) |
| 3 | Create the `flashcards` and `quiz_results` tables with processing status fields, and develop an administrative interface for managing Flashcard content. | 29/06/2026 | 30/06/2026 | |
| 4 | Implement content status management (Pending Approval/Approved) and trigger AWS Lambda to update the search index after approval. | 01/06/2026 | 02/06/2026 | |
| 5 | Create a Presigned URL API for Amazon S3 to allow users to upload learning images and audio files, and configure CloudFront URLs for content delivery. | 02/06/2026 | 03/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Review the technical documentation, finalize the Entity Relationship Diagram (ERD) for users, flashcards, and quiz results, and prepare for Week 12. | 03/06/2026 | 05/06/2026 | [Entity Relationship Diagram Design Guide](https://lucid.co/diagram/erd/tutorial) |

### Achievements for Week 11:

* Successfully completed the design of all APIs for the Flashcard and Quiz features, including a robust input validation mechanism to ensure data integrity.

* Completed the design of the `flashcards` and `quiz_results` tables in PostgreSQL, while successfully developing an administrative dashboard for moderating learning content.

* Successfully implemented the content approval workflow (Pending Approval/Approved) and integrated AWS Lambda to automatically update the search index immediately after content approval.

* Implemented the Amazon S3 Presigned URL mechanism integrated with CloudFront, enabling users to securely and efficiently upload and access learning resources, including images and audio files.

* Finalized the Entity Relationship Diagram (ERD) for key entities, including users, flashcards, and learning results, providing a solid foundation for the subsequent development phases.
