+++
title = "Spring Boot Actuator"
description = "Spring Boot Actuator"
date = "2019-12-12"
categories = [ "example", "configuration" ]
tags = [
#    "example",
#    "hugo",
#    "toml"
]
+++

The spring-boot-actuator module provides all of Spring Boot’s production-ready features. The simplest way to enable the features is to add a dependency to the spring-boot-starter-actuator ‘Starter’.

To add the actuator to a Maven based project, add the following ‘Starter’ dependency:

<div >
<pre class="highlightjs highlight"><code class="language-xml hljs" data-lang="xml">&lt;dependencies&gt;
    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-actuator&lt;/artifactId&gt;
    &lt;/dependency&gt;
&lt;/dependencies&gt;</code></pre>
</div>

<div class="paragraph">
<p>For Gradle, use the following declaration:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="highlightjs highlight"><code class="language-groovy hljs" data-lang="groovy">dependencies {
    compile("org.springframework.boot:spring-boot-starter-actuator")
}</code></pre>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="production-ready-endpoints"><a class="anchor" href="#production-ready-endpoints"></a>Endpoints</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Actuator endpoints let you monitor and interact with your application.
Spring Boot includes a number of built-in endpoints and lets you add your own.
For example, the <code>health</code> endpoint provides basic application health information.</p>
</div>
<div class="paragraph">
<p>Each individual endpoint can be <a href="#production-ready-endpoints-enabling-endpoints">enabled or disabled</a>.
This controls whether or not the endpoint is created and its bean exists in the application context.
To be remotely accessible an endpoint also has to be <a href="#production-ready-endpoints-exposing-endpoints">exposed via JMX or HTTP</a>.
Most applications choose HTTP, where the ID of the endpoint along with a prefix of <code>/actuator</code> is mapped to a URL.
For example, by default, the <code>health</code> endpoint is mapped to <code>/actuator/health</code>.</p>
</div>
<div class="paragraph">
<p>The following technology-agnostic endpoints are available:</p>
</div>
<table class="tableblock frame-all grid-all stretch">
<colgroup>
<col style="width: 28.5714%;">
<col style="width: 71.4286%;">
</colgroup>
<thead>
<tr>
<th class="tableblock halign-left valign-top">ID</th>
<th class="tableblock halign-left valign-top">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>auditevents</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Exposes audit events information for the current application.
Requires an <code>AuditEventRepository</code> bean.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>beans</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Displays a complete list of all the Spring beans in your application.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>caches</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Exposes available caches.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>conditions</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Shows the conditions that were evaluated on configuration and auto-configuration classes and the reasons why they did or did not match.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>configprops</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Displays a collated list of all <code>@ConfigurationProperties</code>.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>env</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Exposes properties from Spring&#8217;s <code>ConfigurableEnvironment</code>.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>flyway</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Shows any Flyway database migrations that have been applied.
Requires one or more <code>Flyway</code> beans.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>health</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Shows application health information.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>httptrace</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Displays HTTP trace information (by default, the last 100 HTTP request-response exchanges).
Requires an <code>HttpTraceRepository</code> bean.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>info</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Displays arbitrary application info.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>integrationgraph</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Shows the Spring Integration graph.
Requires a dependency on <code>spring-integration-core</code>.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>loggers</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Shows and modifies the configuration of loggers in the application.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>liquibase</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Shows any Liquibase database migrations that have been applied.
Requires one or more <code>Liquibase</code> beans.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>metrics</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Shows &#8216;metrics&#8217; information for the current application.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>mappings</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Displays a collated list of all <code>@RequestMapping</code> paths.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>scheduledtasks</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Displays the scheduled tasks in your application.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>sessions</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Allows retrieval and deletion of user sessions from a Spring Session-backed session store.
Requires a Servlet-based web application using Spring Session.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>shutdown</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Lets the application be gracefully shutdown.
Disabled by default.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>threaddump</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Performs a thread dump.</p></td>
</tr>
</tbody>
</table>
<div class="paragraph">
<p>If your application is a web application (Spring MVC, Spring WebFlux, or Jersey), you can use the following additional endpoints:</p>
</div>
<table class="tableblock frame-all grid-all stretch">
<colgroup>
<col style="width: 28.5714%;">
<col style="width: 71.4286%;">
</colgroup>
<thead>
<tr>
<th class="tableblock halign-left valign-top">ID</th>
<th class="tableblock halign-left valign-top">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>heapdump</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Returns an <code>hprof</code> heap dump file.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>jolokia</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Exposes JMX beans over HTTP (when Jolokia is on the classpath, not available for WebFlux).
Requires a dependency on <code>jolokia-core</code>.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>logfile</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Returns the contents of the logfile (if <code>logging.file.name</code> or <code>logging.file.path</code> properties have been set).
Supports the use of the HTTP <code>Range</code> header to retrieve part of the log file&#8217;s content.</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><code>prometheus</code></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Exposes metrics in a format that can be scraped by a Prometheus server.
Requires a dependency on <code>micrometer-registry-prometheus</code>.</p></td>
</tr>
</tbody>
</table>





