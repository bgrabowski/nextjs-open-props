# Page Sections Reference

Patterns for full-width page sections commonly used in marketing sites and landing pages. Each pattern includes the TypeScript component and its CSS Module. These are designed to be composed together to build complete pages.

## Table of Contents

1. [Section Wrapper (base)](#section-wrapper)
2. [Hero Section](#hero-section)
3. [Features Grid](#features-grid)
4. [Testimonials](#testimonials)
5. [CTA Band](#cta-band)
6. [Pricing Table](#pricing-table)
7. [FAQ Section](#faq-section)
8. [Stats / Numbers](#stats-section)
9. [Logo Cloud](#logo-cloud)
10. [Contact Form](#contact-form)

---

## Section Wrapper

A reusable wrapper that all sections should use for consistent spacing and max-width.

### section.tsx

```tsx
import styles from './section.module.css';
import { cx } from '@/lib/classnames';

interface SectionProps extends React.HTMLAttributes<HTMLElement> {
  alternate?: boolean;
}

export function Section({ alternate, className, children, ...props }: SectionProps) {
  return (
    <section className={cx(styles.section, alternate && styles.alt, className)} {...props}>
      <div className={styles.container}>
        {children}
      </div>
    </section>
  );
}

interface SectionHeaderProps {
  title: string;
  description?: string;
  align?: 'center' | 'left';
  className?: string;
}

export function SectionHeader({ title, description, align = 'center', className }: SectionHeaderProps) {
  return (
    <div className={cx(styles.header, align === 'left' && styles.headerLeft, className)}>
      <h2 className={styles.title}>{title}</h2>
      {description && <p className={styles.description}>{description}</p>}
    </div>
  );
}
```

### section.module.css

```css
.section {
  padding: var(--size-10) var(--size-4);
}

@media (min-width: 768px) {
  .section {
    padding: var(--size-12) var(--size-6);
  }
}

.alt {
  background: var(--color-surface-1);
}

.container {
  max-width: min(1280px, 100% - var(--size-8));
  margin-inline: auto;
}

.header {
  max-width: 680px;
  margin-inline: auto;
  text-align: center;
  margin-bottom: var(--size-8);
}

.headerLeft {
  margin-inline: 0;
  text-align: left;
}

.title {
  font-size: var(--font-size-5);
  font-weight: var(--font-weight-7);
  color: var(--color-text-1);
  margin-bottom: var(--size-3);
}

@media (min-width: 768px) {
  .title { font-size: var(--font-size-6); }
}

.description {
  font-size: var(--font-size-2);
  color: var(--color-text-2);
  max-width: 560px;
  margin-inline: auto;
}

.headerLeft .description {
  margin-inline: 0;
}
```

---

## Hero Section

### hero.tsx

```tsx
import Link from 'next/link';
import styles from './hero.module.css';
import { Button } from '@/components/ui/button';

interface HeroProps {
  headline: string;
  subheadline: string;
  primaryCta: { label: string; href: string };
  secondaryCta?: { label: string; href: string };
}

export function Hero({ headline, subheadline, primaryCta, secondaryCta }: HeroProps) {
  return (
    <section className={styles.hero}>
      <div className={styles.container}>
        <div className={styles.content}>
          <h1 className={styles.headline}>{headline}</h1>
          <p className={styles.subheadline}>{subheadline}</p>
          <div className={styles.actions}>
            <Button size="lg" asChild>
              <Link href={primaryCta.href}>{primaryCta.label}</Link>
            </Button>
            {secondaryCta && (
              <Button variant="outline" size="lg" asChild>
                <Link href={secondaryCta.href}>{secondaryCta.label}</Link>
              </Button>
            )}
          </div>
        </div>
      </div>
    </section>
  );
}
```

### hero.module.css

```css
.hero {
  padding: var(--size-12) var(--size-4);
  min-height: 70vh;
  display: flex;
  align-items: center;
}

@media (min-width: 768px) {
  .hero { padding: var(--size-12) var(--size-6); }
}

.container {
  max-width: min(1280px, 100% - var(--size-8));
  margin-inline: auto;
  width: 100%;
}

.content {
  max-width: 720px;
}

.headline {
  font-size: var(--font-size-6);
  font-weight: var(--font-weight-9);
  color: var(--color-text-1);
  line-height: var(--font-lineheight-0);
  margin-bottom: var(--size-4);
  text-wrap: balance;
}

@media (min-width: 768px) {
  .headline { font-size: var(--font-size-7); }
}

@media (min-width: 1024px) {
  .headline { font-size: var(--font-size-8); }
}

.subheadline {
  font-size: var(--font-size-2);
  color: var(--color-text-2);
  line-height: var(--font-lineheight-3);
  margin-bottom: var(--size-6);
  max-width: 560px;
}

@media (min-width: 768px) {
  .subheadline { font-size: var(--font-size-3); }
}

.actions {
  display: flex;
  flex-wrap: wrap;
  gap: var(--size-3);
}
```

---

## Features Grid

### features.tsx

```tsx
import { Section, SectionHeader } from './section';
import styles from './features.module.css';

interface Feature {
  icon: React.ReactNode;
  title: string;
  description: string;
}

interface FeaturesProps {
  title: string;
  description?: string;
  features: Feature[];
  columns?: 2 | 3 | 4;
}

export function Features({ title, description, features, columns = 3 }: FeaturesProps) {
  return (
    <Section>
      <SectionHeader title={title} description={description} />
      <div className={styles.grid} data-columns={columns}>
        {features.map((feature, i) => (
          <div key={i} className={styles.feature}>
            <div className={styles.icon}>{feature.icon}</div>
            <h3 className={styles.featureTitle}>{feature.title}</h3>
            <p className={styles.featureDescription}>{feature.description}</p>
          </div>
        ))}
      </div>
    </Section>
  );
}
```

### features.module.css

```css
.grid {
  display: grid;
  gap: var(--size-6);
  grid-template-columns: 1fr;
}

@media (min-width: 768px) {
  .grid[data-columns="2"] { grid-template-columns: repeat(2, 1fr); }
  .grid[data-columns="3"] { grid-template-columns: repeat(2, 1fr); }
  .grid[data-columns="4"] { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1024px) {
  .grid[data-columns="3"] { grid-template-columns: repeat(3, 1fr); }
  .grid[data-columns="4"] { grid-template-columns: repeat(4, 1fr); }
}

.feature {
  text-align: center;
  padding: var(--size-4);
}

.icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: var(--size-9);
  height: var(--size-9);
  border-radius: var(--radius-round);
  background: var(--color-primary-subtle);
  color: var(--color-primary);
  margin-bottom: var(--size-3);
  font-size: var(--font-size-3);
}

.featureTitle {
  font-size: var(--font-size-2);
  font-weight: var(--font-weight-6);
  color: var(--color-text-1);
  margin-bottom: var(--size-2);
}

.featureDescription {
  font-size: var(--font-size-1);
  color: var(--color-text-2);
  max-width: 320px;
  margin-inline: auto;
}
```

---

## Testimonials

### testimonials.tsx

```tsx
import { Section, SectionHeader } from './section';
import styles from './testimonials.module.css';

interface Testimonial {
  quote: string;
  name: string;
  title: string;
  company?: string;
}

interface TestimonialsProps {
  title: string;
  description?: string;
  testimonials: Testimonial[];
}

export function Testimonials({ title, description, testimonials }: TestimonialsProps) {
  return (
    <Section alternate>
      <SectionHeader title={title} description={description} />
      <div className={styles.grid}>
        {testimonials.map((t, i) => (
          <blockquote key={i} className={styles.card}>
            <p className={styles.quote}>"{t.quote}"</p>
            <footer className={styles.attribution}>
              <cite className={styles.name}>{t.name}</cite>
              <span className={styles.role}>
                {t.title}{t.company && `, ${t.company}`}
              </span>
            </footer>
          </blockquote>
        ))}
      </div>
    </Section>
  );
}
```

### testimonials.module.css

```css
.grid {
  display: grid;
  gap: var(--size-5);
  grid-template-columns: 1fr;
}

@media (min-width: 768px) {
  .grid { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1024px) {
  .grid { grid-template-columns: repeat(3, 1fr); }
}

.card {
  background: var(--color-bg);
  border: var(--border-size-1) solid var(--color-border);
  border-radius: var(--radius-2);
  padding: var(--size-5);
  display: flex;
  flex-direction: column;
  gap: var(--size-4);
}

.quote {
  font-size: var(--font-size-1);
  color: var(--color-text-2);
  line-height: var(--font-lineheight-3);
  font-style: italic;
  flex: 1;
}

.attribution {
  display: flex;
  flex-direction: column;
  gap: var(--size-1);
}

.name {
  font-style: normal;
  font-weight: var(--font-weight-6);
  color: var(--color-text-1);
  font-size: var(--font-size-0);
}

.role {
  font-size: var(--font-size-00);
  color: var(--color-text-3);
}
```

---

## CTA Band

### cta-band.tsx

```tsx
import Link from 'next/link';
import styles from './cta-band.module.css';
import { Button } from '@/components/ui/button';

interface CtaBandProps {
  headline: string;
  description?: string;
  cta: { label: string; href: string };
}

export function CtaBand({ headline, description, cta }: CtaBandProps) {
  return (
    <section className={styles.band}>
      <div className={styles.container}>
        <div className={styles.content}>
          <h2 className={styles.headline}>{headline}</h2>
          {description && <p className={styles.description}>{description}</p>}
        </div>
        <Button size="lg" asChild>
          <Link href={cta.href}>{cta.label}</Link>
        </Button>
      </div>
    </section>
  );
}
```

### cta-band.module.css

```css
.band {
  background: var(--color-primary);
  padding: var(--size-10) var(--size-4);
}

.container {
  max-width: min(1280px, 100% - var(--size-8));
  margin-inline: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: var(--size-5);
}

@media (min-width: 768px) {
  .container {
    flex-direction: row;
    justify-content: space-between;
    text-align: left;
  }
}

.headline {
  font-size: var(--font-size-4);
  font-weight: var(--font-weight-7);
  color: var(--color-primary-text);
}

.description {
  font-size: var(--font-size-1);
  color: var(--color-primary-text);
  opacity: 0.85;
  margin-top: var(--size-2);
}
```

---

## Pricing Table

### pricing.tsx

```tsx
import Link from 'next/link';
import { Section, SectionHeader } from './section';
import { Button } from '@/components/ui/button';
import styles from './pricing.module.css';
import { cx } from '@/lib/classnames';

interface PricingTier {
  name: string;
  price: string;
  period?: string;
  description: string;
  features: string[];
  cta: { label: string; href: string };
  highlighted?: boolean;
}

interface PricingProps {
  title: string;
  description?: string;
  tiers: PricingTier[];
}

export function Pricing({ title, description, tiers }: PricingProps) {
  return (
    <Section>
      <SectionHeader title={title} description={description} />
      <div className={styles.grid}>
        {tiers.map((tier) => (
          <div key={tier.name} className={cx(styles.card, tier.highlighted && styles.highlighted)}>
            {tier.highlighted && <div className={styles.badge}>Most Popular</div>}
            <h3 className={styles.tierName}>{tier.name}</h3>
            <div className={styles.price}>
              <span className={styles.amount}>{tier.price}</span>
              {tier.period && <span className={styles.period}>/{tier.period}</span>}
            </div>
            <p className={styles.tierDescription}>{tier.description}</p>
            <ul className={styles.features}>
              {tier.features.map((f, i) => (
                <li key={i} className={styles.featureItem}>✓ {f}</li>
              ))}
            </ul>
            <Button
              variant={tier.highlighted ? 'primary' : 'outline'}
              size="lg"
              className={styles.cta}
              asChild
            >
              <Link href={tier.cta.href}>{tier.cta.label}</Link>
            </Button>
          </div>
        ))}
      </div>
    </Section>
  );
}
```

### pricing.module.css

```css
.grid {
  display: grid;
  gap: var(--size-5);
  grid-template-columns: 1fr;
  align-items: start;
}

@media (min-width: 768px) {
  .grid { grid-template-columns: repeat(3, 1fr); }
}

.card {
  position: relative;
  background: var(--color-bg);
  border: var(--border-size-1) solid var(--color-border);
  border-radius: var(--radius-2);
  padding: var(--size-6);
  display: flex;
  flex-direction: column;
  gap: var(--size-3);
}

.highlighted {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 1px var(--color-primary);
}

.badge {
  position: absolute;
  top: calc(var(--size-3) * -1);
  left: 50%;
  transform: translateX(-50%);
  background: var(--color-primary);
  color: var(--color-primary-text);
  font-size: var(--font-size-00);
  font-weight: var(--font-weight-6);
  padding: var(--size-1) var(--size-3);
  border-radius: var(--radius-round);
  white-space: nowrap;
}

.tierName {
  font-size: var(--font-size-2);
  font-weight: var(--font-weight-6);
  color: var(--color-text-1);
}

.price {
  display: flex;
  align-items: baseline;
  gap: var(--size-1);
}

.amount {
  font-size: var(--font-size-6);
  font-weight: var(--font-weight-7);
  color: var(--color-text-1);
  line-height: 1;
}

.period {
  font-size: var(--font-size-1);
  color: var(--color-text-3);
}

.tierDescription {
  font-size: var(--font-size-0);
  color: var(--color-text-2);
}

.features {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: var(--size-2);
  margin: var(--size-3) 0;
  flex: 1;
}

.featureItem {
  font-size: var(--font-size-0);
  color: var(--color-text-2);
}

.cta {
  width: 100%;
}
```

---

## Stats Section

### stats.tsx

```tsx
import { Section } from './section';
import styles from './stats.module.css';

interface Stat {
  value: string;
  label: string;
}

interface StatsProps {
  stats: Stat[];
}

export function Stats({ stats }: StatsProps) {
  return (
    <Section alternate>
      <div className={styles.grid}>
        {stats.map((stat, i) => (
          <div key={i} className={styles.stat}>
            <div className={styles.value}>{stat.value}</div>
            <div className={styles.label}>{stat.label}</div>
          </div>
        ))}
      </div>
    </Section>
  );
}
```

### stats.module.css

```css
.grid {
  display: grid;
  gap: var(--size-6);
  grid-template-columns: repeat(2, 1fr);
  text-align: center;
}

@media (min-width: 768px) {
  .grid { grid-template-columns: repeat(4, 1fr); }
}

.stat {
  display: flex;
  flex-direction: column;
  gap: var(--size-1);
}

.value {
  font-size: var(--font-size-6);
  font-weight: var(--font-weight-7);
  color: var(--color-primary);
  line-height: 1;
}

.label {
  font-size: var(--font-size-0);
  color: var(--color-text-3);
  font-weight: var(--font-weight-5);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
```

---

## Logo Cloud

### logo-cloud.tsx

```tsx
import Image from 'next/image';
import { Section } from './section';
import styles from './logo-cloud.module.css';

interface Logo {
  src: string;
  alt: string;
  width: number;
  height: number;
}

interface LogoCloudProps {
  title?: string;
  logos: Logo[];
}

export function LogoCloud({ title, logos }: LogoCloudProps) {
  return (
    <Section>
      {title && <p className={styles.title}>{title}</p>}
      <div className={styles.grid}>
        {logos.map((logo, i) => (
          <div key={i} className={styles.logoWrapper}>
            <Image
              src={logo.src}
              alt={logo.alt}
              width={logo.width}
              height={logo.height}
              className={styles.logo}
            />
          </div>
        ))}
      </div>
    </Section>
  );
}
```

### logo-cloud.module.css

```css
.title {
  text-align: center;
  font-size: var(--font-size-0);
  font-weight: var(--font-weight-5);
  color: var(--color-text-3);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: var(--size-6);
}

.grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: var(--size-7);
}

.logoWrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo {
  opacity: 0.5;
  filter: grayscale(100%);
  transition: opacity 0.2s var(--ease-3), filter 0.2s var(--ease-3);
  max-height: 32px;
  width: auto;

  &:hover {
    opacity: 1;
    filter: grayscale(0%);
  }
}
```

---

## Contact Form

Client component for form state management.

### contact-form.tsx

```tsx
'use client';

import { useState } from 'react';
import { Section, SectionHeader } from './section';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';
import { Button } from '@/components/ui/button';
import styles from './contact-form.module.css';

export function ContactForm() {
  const [status, setStatus] = useState<'idle' | 'loading' | 'success' | 'error'>('idle');

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setStatus('loading');
    // Replace with your form handler
    try {
      await new Promise((r) => setTimeout(r, 1000));
      setStatus('success');
    } catch {
      setStatus('error');
    }
  };

  return (
    <Section>
      <SectionHeader
        title="Get in Touch"
        description="Have a question or want to work together? Drop us a message."
      />
      <div className={styles.formWrapper}>
        {status === 'success' ? (
          <div className={styles.successMessage}>
            <p>Thanks for reaching out! We'll get back to you soon.</p>
          </div>
        ) : (
          <form onSubmit={handleSubmit} className={styles.form}>
            <div className={styles.row}>
              <Input label="Name" name="name" required />
              <Input label="Email" name="email" type="email" required />
            </div>
            <Input label="Subject" name="subject" />
            <Textarea label="Message" name="message" rows={5} required />
            <Button type="submit" size="lg" disabled={status === 'loading'}>
              {status === 'loading' ? 'Sending...' : 'Send Message'}
            </Button>
            {status === 'error' && (
              <p className={styles.errorText}>Something went wrong. Please try again.</p>
            )}
          </form>
        )}
      </div>
    </Section>
  );
}
```

### contact-form.module.css

```css
.formWrapper {
  max-width: 640px;
  margin-inline: auto;
}

.form {
  display: flex;
  flex-direction: column;
  gap: var(--size-4);
}

.row {
  display: grid;
  gap: var(--size-4);
  grid-template-columns: 1fr;
}

@media (min-width: 768px) {
  .row { grid-template-columns: repeat(2, 1fr); }
}

.successMessage {
  text-align: center;
  padding: var(--size-8);
  background: var(--color-success-subtle);
  border-radius: var(--radius-2);
  color: var(--color-success);
  font-weight: var(--font-weight-5);
}

.errorText {
  font-size: var(--font-size-0);
  color: var(--color-destructive);
}
```
