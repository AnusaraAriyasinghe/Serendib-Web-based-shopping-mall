# Serendib Central Shopping Mall

A web-based, multi-merchant shopping mall: merchants run their own shops, customers browse, buy and review across all of them, and platform employees approve merchants and handle support. Built as a six-person group project for **SE2030 – Software Engineering** at SLIIT (Year 2, Semester 1, 2026).

**Stack:** Java 17 · Spring Boot · Spring Security · Spring Data JPA · JSP/JSTL · MySQL · Maven

> This is my personal copy of our group repository, with the full commit history kept. My own commits are listed below. Everyone else's work is credited to them under [Team and credits](#team-and-credits).

## Features

| Module | What it does |
|---|---|
| User & stakeholder management | Customer and merchant registration, merchant approval, role-based login, profiles |
| Product catalogue | Categories, merchant product listings, browsing and sorting |
| Shopping cart | Cart per customer, price breakdown with discounts |
| Order & payment | Checkout across several merchants, per-item order status, payment methods |
| Feedback & ratings | Product reviews, merchant responses, automatic flagging of low-rated products |
| Customer support & inquiries | Inquiries tied to an order or product, threaded responses, notifications, FAQ |

The project uses all five design patterns taught in the course: Observer, Strategy, Factory, Singleton and Decorator. See [Docs/design-patterns/](Docs/design-patterns/README.md).

## My contribution

I'm **Anusara Ariyasinghe (Ariyasinghe P.A.D.A.J.)**. I was the team's **Scrum Master**. Besides my own module, I built the frontend and the shared configuration, and I put the six modules together into one application.

### 1. Customer Support & Inquiry module (my assigned module)
Customers raise inquiries about an order or product. Employees and merchants reply in a thread, and the status moves from `OPEN` to `IN_PROGRESS` to `RESOLVED`. The module also has an FAQ page. It uses the **Observer pattern** to notify the customer, the assigned employee and the merchant when an inquiry changes.

- `src/main/java/com/sliit/se2030/mall/support/`: controllers, entities, repositories, services
- `src/main/java/com/sliit/se2030/mall/support/observer/`: `InquirySubject`, `InquiryObserver`, `InquiryEventPublisher`, and the customer, employee and merchant notifiers
- `src/main/webapp/WEB-INF/views/support/`: submit inquiry, my inquiries, the employee and merchant inquiry lists, inquiry thread, FAQ
- [Docs/design-patterns/01-support-observer.md](Docs/design-patterns/01-support-observer.md)

### 2. Frontend (all modules)
I designed and built the UI for the whole application, every page in every module.

- `src/main/webapp/WEB-INF/views/`: all JSP views, plus the shared fragments (`common/layout-header.jsp`, `layout-footer.jsp`, `product-tile.jspf`, `price-breakdown.jspf`, `shop-logo.jspf`)
- `src/main/resources/static/css/serendib.css` and `static/js/serendib.js`: the Serendib theme and its client-side behaviour
- `src/main/resources/static/images/logos/`: logos for the sample shops

### 3. Shared configuration, security and common code
- `config/SecurityConfig.java`: role-based access rules, form login, logout, session limits
- `security/`: `AppUserDetailsService`, `AppUserPrincipal`, `RoleBasedAuthenticationSuccessHandler`
- `config/DataSeedConfig.java` and `config/SampleCatalogData.java`: seed data for local development
- `common/`: `BaseEntity`, `GlobalExceptionHandler`, the custom exceptions, `CurrentUserProvider`, and the controller advice classes for the cart badge and admin notifications

### 4. Integration and other features
- Set up the project structure and the conventions every module follows
- Merged the team's feature branches and fixed integration bugs: merchant account verification, product delete, product image URLs
- Added category management and FAQ management
- Customer profile edit and delete: `user/controller/CustomerProfileController.java`, `user/service/CustomerProfileService.java`, `user/dto/CustomerProfileForm.java`, `views/user/customer-profile.jsp`, `CustomerProfileServiceTest`
- Applied the design patterns in the other modules together with their owners, and wrote the viva guide in [Docs/design-patterns/](Docs/design-patterns/README.md)

## Team and credits

This is group work. Each module's business logic belongs to the member listed here.

| Member | Role | Module |
|---|---|---|
| Bandara N.V. | Product Owner | Product Catalogue Management |
| **Ariyasinghe P.A.D.A.J.** | **Scrum Master** | **Customer Support & Inquiry**, frontend, shared config |
| Vaas W.P.S.D. | Developer | Shopping Cart Handling (`cart/`) |
| Maliduwa M.G.B.B. | Developer | Order & Payment Handling (`order/`) |
| Weerawickrama K.K. | Developer | Product Review & Rating (`feedback/`) |
| Neluvinda T.M.D.N. | Developer | Merchant & User Management (`user/`) |

Each member's original submission is kept under [uploads/](uploads/), with their own `NOTES.md`. Some members' work reached `main` through my integration commits, so the commit history doesn't show every contribution. The table above is the actual split of the work.

## Results

- All six modules work end to end against a running MySQL database.
- `mvn test` runs 31 tests in 8 test classes, and all pass. The tests cover the design-pattern classes (payment strategy factory, cart price decorator, catalogue sort strategy, rating strategy, user factory) as well as the user controller and services.

## Running locally

**Prerequisites:** JDK 17, Maven 3.9+, and a running MySQL server.

1. Clone the repo:
   ```bash
   git clone https://github.com/AnusaraAriyasinghe/Serendib-Web-based-shopping-mall.git
   cd Serendib-Web-based-shopping-mall
   ```
2. Set your MySQL credentials as environment variables. If you don't, the app uses `root` with an empty password.
   ```bash
   export DB_USERNAME=root
   export DB_PASSWORD=your_password
   ```
   You don't need to create the database. The app creates `shopping_mall` on first run.
3. Run the app:
   ```bash
   mvn spring-boot:run
   ```
4. Open http://localhost:8081. A demo employee account is created on first start, `admin@mall.local` / `admin123`, for local use only.

Run the tests with `mvn test`.

Code layout and team conventions are in [Docs/developer-guide.md](Docs/developer-guide.md).

## Next steps

- Commit the Maven wrapper (`.mvn/wrapper/`) so `./mvnw` works without a local Maven install
- Restrict who can reopen a resolved inquiry (for example, only the assigned employee)
- Make the Observer notifiers send real email or in-app notifications (right now they only write a log line)
- Add more tests for the support, cart and order services
- Deploy a live demo
