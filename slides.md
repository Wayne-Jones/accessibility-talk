---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Building Accessible Components - Best Practices and Business Buy-In
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: fade-out
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Building Accessible Components

Best Practices and Business Buy-In

<br>
<br>

Wayne Jones | Senior Software Engineer at Capital One

Commit Your Code 2025


<!--
I'm Wayne Jones, I'm a Senior Software Engineer at Capital One and
an aspiring developer advocate. I have over 12 years of experience
as a software developer and I am an advocate for making accessibility a priority in the web development process.
-->

---
transition: fade-out
---

# Why Accessibility is Important

- Inaccessible websites means that people with disabilities are denied equal access to get information.
- Accessibility helps everyone: Screen Readers, Keyboard users, users with temporary impairments (e.g. a broken arm, eye strain).
- Companies can be sued for not maintaining an accessible website.

<br>
<br>

<!--
Did you know that according to the CDC 1 in 4 adults in the US live with a disability? And accessibility isn't just for permanent disabilities - think about someone with a broken arm or have eye strain from looking at a screen for too long. Accessibility helps everyone.

Having an inaccessible website excludes people just as much as a physical location that has steps at an entrance. In a way excluding
a person digitally is in a way a form of discrimination.

In addition, in the United States, companies could be held liable for failing to create an accessible website. We are seeing more and more companies being sued for accessibility violations.
-->
---
transition: fade-out
class: text-center
layout: cover
---

# The Principles of Web Accessibility

## POUR Framework

<!--
When thinking about Web Accessibility, it's good to keep in mind the four principles of accessibility known as the POUR framework.
-->
---
transition: fade-out
---

# Perceivable

- Provide text alternatives for non-text content (e.g., alt attributes for images)

```html
<img src="example.jpg" alt="Description of the image" />
```

- Ensure sufficient color contrast between text and background
  - Example: Use tools like Contrast Checker

<!--
Information and user interface components must be presentable to users in ways they can perceive.

This means that users must be able to perceive the information being presented (it can't be invisible to all of their senses).

Examples: Text alternatives for non-text content, captions and other alternatives for multimedia, adaptable; distinguishable (e.g. color contrast)
-->

---
transition: fade-out
---
# Operable

- Make all functionality available from a keyboard (e.g., using Tab key to navigate, avoid mouse-only interactions)

```jsx
 <button 
    onClick={handleClick} 
    onKeyDown={(e) => e.key === 'Enter' && handleClick()}>
      Submit
 </button>
```

- Include skip navigation links for quick access to main content

```jsx
  <a href="#main-content" class="skip-link">Skip to main content</a>
  ...
  <main id="main-content">
    <!-- Main content goes here -->
  </main>
```
<!--
User interface components and navigation must be operable.

This means that users must be able to operate the interface (the interface cannot require interaction that a user cannot perform).

Examples: Keyboard accessible, avoiding time-limited interactions, and providing ways to help users navigate, find content, and determine where they are on the page
-->
---
transition: fade-out
---
# Understandable
- Use clear and consistent labels and instructions

```jsx
  <label for="email">Email Address:</label>
  <input type="email" id="email" name="email" required />
```

- Provide helpful error messages

```jsx
  <label for="email">Email Address:</label>
  <input type="email" id="email" name="email" required />
  <p role="alert">Please enter a valid email address.</p>
```
<!--
Information and the operation of user interface must be understandable.

This means that users must be able to understand the information as well as the operation of the user interface (the content or operation cannot be beyond their understanding).

Examples: Using labels for input fields, providing clear and concise error messages, and using plain language.
-->
---
transition: fade-out
---
# Robust
- Use semantic HTML elements (e.g., headings, lists, buttons) to convey meaning and structure

```html
  <nav>
    <ul>
      <li><a href="#home">Home</a></li>
      <li><a href="#about">About</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>
```

- Ensure compatibility with assistive technologies (e.g., screen readers)

```jsx
<button aria-label="Close" onClick={handleClose}>
  &times;
</button>
```
<!--
Content must be robust enough that it can be interpreted reliably by a wide variety of user agents, including assistive technologies.

This means that users must be able to access the content as technologies advance (as technologies and user agents evolve, the content should remain accessible).

Examples: Using semantic HTML, ensuring compatibility with current and future user agents, including assistive technologies. Using ARIA roles to enhance accessibility where native html falls short.
-->
---
transition: fade-out
class: text-center
---
# Practical Techniques in React
<!--
Now that we've covered the four principles of accessibility, let's dive into some practical techniques you can use in your React applications to improve accessibility. While this is in React, these techniques can be applied to any web development framework or library.
-->
---
transition: fade-out
---
# Semantic HTML

```jsx
  // Avoid using generic elements like divs
  <button onClick={handleClick}>Click Me</button>

  // Instead of this
  <div role="button" onClick={handleClick} tabIndex="0">Click Me</div>
```
<!--
Using semantic HTML elements is crucial for accessibility. For
example, use button tag instead of a div tag with
a role of button. This ensures that assistive technologies can
correctly identify the element and provide the appropriate
functionality.
-->
---
transition: fade-out
---
# Focus Management

```jsx
  import { useRef, useEffect } from 'react';

  function Modal({ isOpen, onClose }) {
    const closeButtonRef = useRef(null);

    useEffect(() => {
      if (isOpen) {
        closeButtonRef.current.focus();
      }
    }, [isOpen]);

    return (
      isOpen && (
        <div role="dialog" aria-labelledby="modal-title">
          <h2 id="modal-title">Modal Title</h2>
          <button ref={closeButtonRef} onClick={onClose}>Close</button>
        </div>
      )
    );
  }
```
<!--
Managing focus is essential for keyboard users. When a modal opens,
set focus to the first interactive element, such as the close
button. This ensures that users can easily interact with the modal
without having to navigate through the entire page.
-->
---
transition: fade-out
---
# ARIA Roles and Attributes

```jsx
  function Dropdown({ label, options }) {
    return (
      <div>
        <button aria-haspopup="listbox" aria-expanded="false">
          {label}
        </button>
        <ul role="listbox" aria-label="Dropdown options">
          {options.map((option, index) => (
            <li key={index} role="option" tabIndex="-1">
              {option}
            </li>
            ))
          }
        </ul>
      </div>
    );
  }
```
<!--
ARIA roles and attributes can enhance accessibility when native HTML
elements fall short. For example, use aria-haspopup and aria-expanded attributes on dropdown buttons to indicate their
state. 

Use role="listbox" for the dropdown menu and role="option" for each item.
-->
---
transition: fade-out
---
# Keyboard Navigation

```jsx
  function Tabs({ tabs }) {
    const [activeTab, setActiveTab] = useState(0);

    const handleKeyDown = (e) => {
      if (e.key === 'ArrowRight') {
        setActiveTab((prev) => (prev + 1) % tabs.length);
      } else if (e.key === 'ArrowLeft') {
        setActiveTab((prev) => (prev - 1 + tabs.length) % tabs.length);
      }
    };

    return (
      <div role="tablist" onKeyDown={handleKeyDown}>
        {tabs.map((tab, index) => (
          <button
            key={index}
            role="tab"
            aria-selected={activeTab === index}
            tabIndex={activeTab === index ? 0 : -1}
            onClick={() => setActiveTab(index)}
          >
            {tab.label}
          </button>
        ))}
        <div role="tabpanel">
          {tabs[activeTab].content}
        </div>
      </div>
    );
  }
```
<!--
Ensure that all interactive elements are reachable and operable via
keyboard. For example, implement keyboard navigation for tab
components using arrow keys to switch between tabs.
-->
---
transition: fade-out
---
# Accessibility Testing Tools

- Automated Tools
  - Axe Browser Extension
  - Lighthouse Accessibility Audit
- Screen Reader Testing
  - VoiceOver (Mac), NVDA (Windows)
- Manual Testing Checklist:
  - Is every interactive action reachable via keyboard?
  - Are ARIA roles used appropriately
  - Is the color contrast sufficient?

<!--
While automated tools can catch some common accessibility issues,
they cannot catch everything. It's important to also conduct manual
testing using screen readers and keyboard navigation to ensure a
fully accessible experience.
-->
---
transition: fade-out
---
# Advocating for Accessibility
- Build the Business Case:
  - Accessibility expands market reach (e.g., aging population)
  - Reduces risk of legal action
  - Demonstrates commitment to inclusivity and corporate social responsibility.
- Align Accessibility with Business Goals:
  - Example: Increased customer satisfaction by improving app usability.
- Start small:
  - Audit a single component
  - Share wins with your team to build momentum
<!--

-->
---
transition: fade-out
layout: center
---
# An Accessibility Success Story

<!--
Worked for a digital agency, Jovia Financial as a client came 
to us in order to make sure that their website (jovia.org) was fully compliant. 
Their website was full of accessibility issues but one area in particular was
the most hard to deal with was the navigation menu.
-->
---
transition: fade-out
layout: center
---
# The Problem

<!--
The Jovia Website CSS was build upon FoundationCSS v6. The navigational bar itself was built from a component from Foundation and it was inaccessible. Even though the website claims that their  I had went on Github to see if other people were aware of this accessibility issue and this is where I found this 4 year old issue at the time.

The maintainer was made aware of the accessibility issue even after a lengthy explaination. They even claimed that the menu is technically correct and the actual fix wouldn't be a priority until Version 7.
-->
---
transition: fade-out
layout: center
---
# The Solution

<!--
So I had 3 different options:
1. Wait for Foundation Version 7
2. Fork Foundation Version 6 and implement the fix myself
3. Scrap the Foundation menu and build the mega menu all from scratch

1. This was a 4 year old issue and it seems like Foundation Version 7 was no where close to being released. (Spoiler alert - 8 years later and Foundation is still on Version 6 and it's barely maintained)
2. Forking foundation version 6 would mean taking on tech debt and having to maintain this source code myself
3. Building the menu from scratch would take time but it would ensure that every element of the menu is accessible
-->
---
transition: fade-out
layout: center
---
# The Outcome

<!--
We ended up going with option 3 to build the mega menu from scratch. I combined using both automated and manual tests in my audit of the Jovia website. This ended up with these results:
1. Improvement in Accessibility Score in Lighthouse, Passed WCAG 2.1 AA standards
2. Scalability - Allow for the ability in the future if they wanted to completely move away from Foundation they can take this component UI with them
3. Business goals were aligned with tech / product goals and it reduced the liability of Jovia from website accessibility-related lawsuits.
-->
---
transition: fade-out
---
# Key Takeaways

- Start small: pilot a high‑impact flow or component.
- Measure and share wins to build momentum.
- Combine automated tests with manual screen‑reader and keyboard audits.
- Invest in reusable accessible components and lightweight process changes for lasting impact.
- Accessibility is both a moral and business win — inclusive products reach more customers and reduce risk.

---
transition: fade-out
layout: center
---
# Call to Action
---
transition: fade-out
layout: statement
---
# Let's collaborate to make accessibility a priority in every project!
---
transition: fade-out
layout: end
---
# Thank You
