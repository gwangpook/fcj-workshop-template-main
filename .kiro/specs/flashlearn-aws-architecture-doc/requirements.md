# Requirements Document: FlashLearn AWS Architecture Documentation

## Introduction

This document specifies the requirements for creating accurate AWS architecture documentation for the FlashLearn application. FlashLearn is an ASP.NET Core 8.0 web application for learning English vocabulary through flashcards with spaced repetition. The architecture documentation must accurately depict the Single-AZ AWS infrastructure deployment, avoiding critical architectural mistakes that misrepresent AWS service usage and network topology.

The documentation will be integrated into the existing workshop structure at `content/5-Workshop/` and will serve as a reference for understanding the complete AWS infrastructure, service interactions, and network flows.

## Glossary

- **Architecture_Documentation_Generator**: The system responsible for creating architecture diagrams and explanatory documentation
- **Diagram_Renderer**: The component that generates visual representations of the AWS architecture (Mermaid or ASCII)
- **Content_Validator**: The component that verifies accuracy of AWS service descriptions and network flows
- **Workshop_Integrator**: The component that integrates documentation into the existing workshop structure
- **Mermaid_Diagram**: A text-based diagram format that can be rendered into visual diagrams
- **Single-AZ_Architecture**: AWS architecture deployed in a single Availability Zone (ap-southeast-1a)
- **Public_Subnet**: Network segment (10.0.1.0/24) with internet connectivity via Internet Gateway
- **Private_Subnet**: Network segment (10.0.2.0/24) without direct internet access
- **VPC**: Virtual Private Cloud (10.0.0.0/16) - isolated network environment in AWS
- **Best_Practice_Note**: Documentation section explaining production-grade improvements beyond basic setup

## Requirements

### Requirement 1: Architecture Diagram Generation

**User Story:** As a workshop participant, I want to see an accurate architecture diagram, so that I understand how AWS services are connected in the FlashLearn application.

#### Acceptance Criteria

1. THE Diagram_Renderer SHALL generate a visual representation of the AWS architecture using Mermaid diagram format
2. THE Diagram_Renderer SHALL depict EC2 instance placement in the Public_Subnet (10.0.1.0/24)
3. THE Diagram_Renderer SHALL depict RDS PostgreSQL placement in the Private_Subnet (10.0.2.0/24)
4. THE Diagram_Renderer SHALL show Amazon S3 as an external service accessible from EC2 via internet
5. THE Diagram_Renderer SHALL show Amazon Cognito as an external authentication service
6. THE Diagram_Renderer SHALL show the VPC containing both Public_Subnet and Private_Subnet in ap-southeast-1a
7. THE Diagram_Renderer SHALL NOT include Application Load Balancer in the diagram
8. THE Diagram_Renderer SHALL NOT include Route 53 in the diagram
9. THE Diagram_Renderer SHALL NOT include NAT Gateway in the diagram
10. THE Diagram_Renderer SHALL show Internet Gateway attached to the VPC for Public_Subnet connectivity

### Requirement 2: Network Flow Documentation

**User Story:** As a workshop participant, I want to understand the network flow between services, so that I can troubleshoot connectivity issues and understand security boundaries.

#### Acceptance Criteria

1. THE Architecture_Documentation_Generator SHALL document the user authentication flow through Amazon Cognito
2. THE Architecture_Documentation_Generator SHALL document the HTTP request flow from users to EC2 via public IP address
3. THE Architecture_Documentation_Generator SHALL document the database connection flow from EC2 to RDS within the VPC
4. THE Architecture_Documentation_Generator SHALL document the S3 access flow from EC2 via internet gateway
5. THE Architecture_Documentation_Generator SHALL specify that EC2 accesses S3 through the internet (not VPC endpoint)
6. THE Architecture_Documentation_Generator SHALL specify that RDS is only accessible from within the VPC
7. THE Architecture_Documentation_Generator SHALL document port numbers and protocols for each connection type

### Requirement 3: AWS Service Description Accuracy

**User Story:** As a workshop participant, I want accurate descriptions of each AWS service, so that I understand the role and purpose of each component.

#### Acceptance Criteria

1. THE Content_Validator SHALL verify that Amazon Cognito description includes JWT token generation functionality
2. THE Content_Validator SHALL verify that EC2 description specifies t3.micro instance type and ASP.NET Core 8.0 runtime
3. THE Content_Validator SHALL verify that RDS description specifies PostgreSQL engine and db.t3.micro instance type
4. THE Content_Validator SHALL verify that S3 description specifies flashcard image storage purpose
5. THE Content_Validator SHALL verify that VPC description includes the CIDR block 10.0.0.0/16
6. THE Content_Validator SHALL verify that Public_Subnet description includes CIDR block 10.0.1.0/24 and Availability Zone ap-southeast-1a
7. THE Content_Validator SHALL verify that Private_Subnet description includes CIDR block 10.0.2.0/24 and Availability Zone ap-southeast-1a

### Requirement 4: Best Practices Documentation

**User Story:** As a workshop participant, I want to know production-grade improvements, so that I can enhance the architecture for real-world deployments.

#### Acceptance Criteria

1. THE Architecture_Documentation_Generator SHALL include a Best_Practice_Note recommending VPC Gateway Endpoint for S3 access
2. THE Architecture_Documentation_Generator SHALL include a Best_Practice_Note recommending Multi-AZ deployment for high availability
3. THE Architecture_Documentation_Generator SHALL include a Best_Practice_Note recommending Application Load Balancer for production traffic distribution
4. THE Architecture_Documentation_Generator SHALL include a Best_Practice_Note recommending NAT Gateway for private subnet outbound connectivity
5. THE Architecture_Documentation_Generator SHALL include a Best_Practice_Note recommending RDS Multi-AZ for database redundancy
6. THE Architecture_Documentation_Generator SHALL include a Best_Practice_Note recommending CloudFront CDN for S3 content delivery
7. THE Architecture_Documentation_Generator SHALL clearly label each best practice as a future improvement, not part of the basic workshop

### Requirement 5: Vietnamese Language Content

**User Story:** As a Vietnamese-speaking workshop participant, I want documentation in Vietnamese, so that I can understand the architecture in my native language.

#### Acceptance Criteria

1. THE Architecture_Documentation_Generator SHALL generate all explanatory text in Vietnamese language
2. THE Architecture_Documentation_Generator SHALL use technical AWS service names in English within Vietnamese text
3. THE Architecture_Documentation_Generator SHALL maintain consistent terminology with existing workshop content
4. THE Architecture_Documentation_Generator SHALL follow the writing style and tone of existing workshop sections
5. WHEN generating Vietnamese content, THE Architecture_Documentation_Generator SHALL use appropriate technical vocabulary matching the existing workshop glossary

### Requirement 6: Workshop Integration

**User Story:** As a workshop maintainer, I want architecture documentation integrated into the workshop structure, so that participants can access it as part of their learning journey.

#### Acceptance Criteria

1. THE Workshop_Integrator SHALL create documentation file in markdown format
2. THE Workshop_Integrator SHALL include Hugo front matter with appropriate weight and chapter settings
3. THE Workshop_Integrator SHALL follow the existing workshop file naming convention
4. THE Workshop_Integrator SHALL place the documentation in the appropriate section of `content/5-Workshop/`
5. WHEN creating the documentation file, THE Workshop_Integrator SHALL ensure compatibility with Hugo static site generator

### Requirement 7: Diagram Clarity and Readability

**User Story:** As a workshop participant, I want a clear and readable diagram, so that I can quickly grasp the architecture without confusion.

#### Acceptance Criteria

1. THE Diagram_Renderer SHALL use distinct visual elements for different AWS service types
2. THE Diagram_Renderer SHALL show network boundaries clearly (VPC, subnets)
3. THE Diagram_Renderer SHALL use directional arrows to indicate data flow and dependencies
4. THE Diagram_Renderer SHALL include IP CIDR blocks in subnet labels
5. THE Diagram_Renderer SHALL group related components logically
6. THE Diagram_Renderer SHALL use consistent color coding or styling for service categories
7. WHEN rendered by Hugo, THE Mermaid_Diagram SHALL display correctly without distortion or overflow

### Requirement 8: Critical Error Prevention

**User Story:** As a workshop maintainer, I want to prevent critical architectural mistakes, so that participants learn correct AWS best practices and service usage.

#### Acceptance Criteria

1. IF the documentation depicts EC2 in Private_Subnet, THEN THE Content_Validator SHALL reject the documentation
2. IF the documentation shows a VPC Endpoint for S3 in the basic architecture, THEN THE Content_Validator SHALL reject the documentation
3. IF the documentation includes ALB without noting it as a best practice improvement, THEN THE Content_Validator SHALL reject the documentation
4. IF the documentation includes Route 53 without noting it as a best practice improvement, THEN THE Content_Validator SHALL reject the documentation
5. IF the documentation depicts Multi-AZ deployment in the basic architecture, THEN THE Content_Validator SHALL reject the documentation
6. IF the documentation shows NAT Gateway in the basic architecture, THEN THE Content_Validator SHALL reject the documentation
7. THE Content_Validator SHALL verify that EC2 has a public IP address for direct internet access

### Requirement 9: Component Relationship Documentation

**User Story:** As a workshop participant, I want to understand how components interact, so that I can configure applications correctly and debug integration issues.

#### Acceptance Criteria

1. THE Architecture_Documentation_Generator SHALL document the authentication flow sequence: User → Cognito → EC2
2. THE Architecture_Documentation_Generator SHALL document the data persistence flow: EC2 → RDS
3. THE Architecture_Documentation_Generator SHALL document the image storage flow: EC2 → S3
4. THE Architecture_Documentation_Generator SHALL specify that EC2 connects to RDS using private IP addresses
5. THE Architecture_Documentation_Generator SHALL specify that EC2 connects to S3 using AWS SDK over HTTPS
6. THE Architecture_Documentation_Generator SHALL document security group requirements for each connection
7. THE Architecture_Documentation_Generator SHALL document IAM role requirements for EC2 to access S3

### Requirement 10: Reference Consistency

**User Story:** As a workshop participant, I want architecture documentation consistent with workshop steps, so that I can cross-reference between sections without confusion.

#### Acceptance Criteria

1. THE Content_Validator SHALL verify that documented VPC CIDR matches section 5.3 VPC setup
2. THE Content_Validator SHALL verify that documented subnet CIDRs match section 5.3 VPC setup
3. THE Content_Validator SHALL verify that documented RDS configuration matches section 5.4 RDS deployment
4. THE Content_Validator SHALL verify that documented EC2 configuration matches section 5.5 EC2 deployment
5. THE Content_Validator SHALL verify that documented S3 configuration matches section 5.6 S3 configuration
6. THE Content_Validator SHALL verify that documented Cognito configuration matches section 5.7 Cognito integration
7. WHEN workshop steps are updated, THE Architecture_Documentation_Generator SHALL maintain consistency with the current workshop structure
