# AWS-Basics
Hands-on AWS learning repository covering cloud computing fundamentals, core AWS services, networking, IAM, storage, and practical labs with documentation.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AWS Architecture Flow</title>
  <style>
    :root {
      --blue: #e8f1ff;
      --blue-border: #c8dcff;
      --border: #d9dee7;
      --text: #172033;
      --muted: #5f6b7a;
      --line: #7c8ba1;
      --surface: #ffffff;
      --page: #f7f9fc;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      padding: 28px 16px;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      color: var(--text);
      background: var(--page);
    }

    .page {
      width: min(900px, 100%);
      margin: 0 auto;
    }

    header {
      text-align: center;
      margin-bottom: 22px;
    }

    h1 {
      margin: 0 0 8px;
      font-size: clamp(1.5rem, 4vw, 2.15rem);
      letter-spacing: -0.035em;
    }

    header p {
      margin: 0;
      color: var(--muted);
      line-height: 1.6;
    }

    .architecture {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 18px;
      padding: clamp(12px, 3vw, 24px);
      box-shadow: 0 12px 32px rgba(26, 43, 70, 0.07);
    }

    .node {
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 14px 16px;
      text-align: center;
      background: #fff;
    }

    .node.primary {
      background: var(--blue);
      border-color: var(--blue-border);
    }

    .node h2, .service h3 {
      margin: 0 0 5px;
      font-size: 1rem;
      line-height: 1.35;
    }

    .node p, .service p {
      margin: 0;
      color: var(--muted);
      font-size: 0.88rem;
      line-height: 1.5;
    }

    .arrow {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 2px;
      padding: 5px 0;
      color: var(--line);
      text-align: center;
      font-size: 1.15rem;
    }

    .arrow small {
      color: var(--muted);
      font-size: 0.72rem;
      line-height: 1.35;
    }

    .backend-label {
      text-align: center;
      color: var(--muted);
      font-size: 0.78rem;
      margin: 0 0 8px;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 12px;
    }

    .grid.data {
      grid-template-columns: repeat(2, minmax(0, 1fr));
      margin-top: 16px;
    }

    .service {
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 14px;
      background: #fff;
      min-width: 0;
    }

    .service.primary {
      background: var(--blue);
      border-color: var(--blue-border);
    }

    .service-icon {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 28px;
      height: 28px;
      margin-bottom: 8px;
      border-radius: 8px;
      background: #f0f4fa;
      color: #31445f;
      font-size: 1rem;
      font-weight: 700;
    }

    .service.primary .service-icon { background: #d7e7ff; }

    .section-note {
      text-align: center;
      color: var(--muted);
      font-size: 0.75rem;
      line-height: 1.5;
      margin: 9px 0 0;
    }

    .support {
      border-top: 1px solid var(--border);
      margin-top: 18px;
      padding-top: 16px;
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 12px;
    }

    .support .service { background: var(--blue); border-color: var(--blue-border); }

    .footnote {
      margin: 14px 2px 0;
      color: var(--muted);
      font-size: 0.76rem;
      line-height: 1.55;
    }

    @media (max-width: 620px) {
      body { padding: 18px 10px; }
      .grid { grid-template-columns: 1fr; }
      .grid.data, .support { grid-template-columns: 1fr; }
      .node, .service { padding: 13px; }
      .architecture { padding: 10px; border-radius: 14px; }
    }
  </style>
</head>
<body>
  <main class="page">
    <header>
      <h1>AWS Application Architecture</h1>
      <p>How a request can travel from a client to an AWS backend</p>
    </header>

    <section class="architecture" aria-label="AWS architecture flow diagram">
      <div class="node primary">
        <h2>1. User / Client</h2>
        <p>Browser, mobile app, or another application sends an HTTP request.</p>
      </div>

      <div class="arrow" aria-hidden="true">↓<small>Resolve the domain name</small></div>

      <div class="node">
        <h2>2. Amazon Route 53</h2>
        <p>DNS service that directs the domain to the appropriate endpoint.</p>
      </div>

      <div class="arrow" aria-hidden="true">↓</div>

      <div class="node">
        <h2>3. Amazon CloudFront</h2>
        <p>CDN that serves cached content when available, or forwards requests to the configured origin.</p>
      </div>

      <div class="arrow" aria-hidden="true">↓<small>For API requests, if configured this way</small></div>

      <div class="node">
        <h2>4. Amazon API Gateway</h2>
        <p>Accepts API calls, applies configured authorization and throttling, and routes requests to a backend integration.</p>
      </div>

      <div class="arrow" aria-hidden="true">↓</div>
      <p class="backend-label">Choose a suitable backend architecture — these are alternatives in many designs.</p>

      <div class="grid">
        <article class="service primary">
          <span class="service-icon" aria-hidden="true">▣</span>
          <h3>Amazon EC2</h3>
          <p>Virtual servers for applications where you need control over the operating system and environment.</p>
        </article>
        <article class="service primary">
          <span class="service-icon" aria-hidden="true">⬡</span>
          <h3>Amazon ECS</h3>
          <p>Runs and manages containerized applications, services, and background workers.</p>
        </article>
        <article class="service primary">
          <span class="service-icon" aria-hidden="true">ϟ</span>
          <h3>AWS Lambda</h3>
          <p>Runs functions in response to API calls, events, or messages without managing servers.</p>
        </article>
      </div>

      <div class="arrow" aria-hidden="true">↓<small>Backend services access these as required</small></div>

      <div class="grid data">
        <article class="service">
          <span class="service-icon" aria-hidden="true">▤</span>
          <h3>Amazon RDS</h3>
          <p>Managed relational databases: tables, relationships, and SQL.</p>
        </article>
        <article class="service">
          <span class="service-icon" aria-hidden="true">▤</span>
          <h3>Amazon DynamoDB</h3>
          <p>Managed NoSQL key-value and document database.</p>
        </article>
        <article class="service">
          <span class="service-icon" aria-hidden="true">▱</span>
          <h3>Amazon S3</h3>
          <p>Object storage for images, files, documents, videos, and backups.</p>
        </article>
        <article class="service">
          <span class="service-icon" aria-hidden="true">⇄</span>
          <h3>Amazon SQS</h3>
          <p>Message queue for decoupled, asynchronous background processing.</p>
        </article>
      </div>

      <div class="support">
        <article class="service">
          <span class="service-icon" aria-hidden="true">◷</span>
          <h3>Amazon CloudWatch</h3>
          <p>Collects logs and metrics, and supports alarms and dashboards.</p>
        </article>
        <article class="service">
          <span class="service-icon" aria-hidden="true">◇</span>
          <h3>AWS IAM</h3>
          <p>Controls which identities can perform actions on AWS resources.</p>
        </article>
      </div>

      <p class="footnote">
        Note: This is a conceptual architecture, not a mandatory request path. Route 53 resolves DNS;
        CloudFront is optional; EC2, ECS, and Lambda are backend choices; RDS, DynamoDB, S3, and SQS
        are used when the application needs them. CloudWatch monitors services, while IAM governs permissions.
      </p>
    </section>
  </main>
</body>
</html>
