# Component Patterns Reference

Ready-to-use patterns for common UI components. Each pattern includes the TypeScript component and its co-located CSS Module. Copy and adapt these when generating components.

## Table of Contents

1. [Button](#button)
2. [Card](#card)
3. [Input](#input)
4. [Textarea](#textarea)
5. [Badge](#badge)
6. [Modal / Dialog](#modal--dialog)
7. [Accordion](#accordion)
8. [Navigation Header](#navigation-header)
9. [Mobile Navigation](#mobile-navigation)
10. [Footer](#footer)
11. [Theme Toggle](#theme-toggle)

---

## Button

### button.tsx

```tsx
import styles from './button.module.css';
import { cx } from '@/lib/classnames';

interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost' | 'destructive';
  size?: 'sm' | 'default' | 'lg';
}

export function Button({
  variant = 'primary',
  size = 'default',
  className,
  children,
  ...props
}: ButtonProps) {
  return (
    <button
      className={cx(
        styles.button,
        styles[variant],
        size !== 'default' && styles[size],
        className,
      )}
      {...props}
    >
      {children}
    </button>
  );
}
```

### button.module.css

```css
.button {
  font-family: var(--font-sans);
  font-size: var(--font-size-1);
  font-weight: var(--font-weight-6);
  line-height: var(--font-lineheight-1);
  padding: var(--size-2) var(--size-5);
  border-radius: var(--radius-2);
  border: var(--border-size-1) solid transparent;
  cursor: pointer;
  transition: background 0.15s var(--ease-3), color 0.15s var(--ease-3),
    border-color 0.15s var(--ease-3), box-shadow 0.15s var(--ease-3);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--size-2);
  white-space: nowrap;
  text-decoration: none;

  &:focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: 2px;
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }
}

/* Variants */

.primary {
  background: var(--color-primary);
  color: var(--color-primary-text);

  &:hover:not(:disabled) { background: var(--color-primary-hover); }
  &:active:not(:disabled) { background: var(--color-primary-active); }
}

.secondary {
  background: var(--color-surface-1);
  color: var(--color-text-1);
  border-color: var(--color-border);

  &:hover:not(:disabled) { background: var(--color-surface-2); }
  &:active:not(:disabled) { background: var(--color-surface-3); }
}

.outline {
  background: transparent;
  color: var(--color-primary);
  border-color: var(--color-primary);

  &:hover:not(:disabled) {
    background: var(--color-primary-subtle);
  }
  &:active:not(:disabled) {
    background: var(--color-surface-2);
  }
}

.ghost {
  background: transparent;
  color: var(--color-text-2);

  &:hover:not(:disabled) { background: var(--color-surface-1); }
  &:active:not(:disabled) { background: var(--color-surface-2); }
}

.destructive {
  background: var(--color-destructive);
  color: var(--color-text-inverse);

  &:hover:not(:disabled) { background: var(--color-destructive-hover); }
}

/* Sizes */

.sm {
  font-size: var(--font-size-0);
  padding: var(--size-1) var(--size-3);
}

.lg {
  font-size: var(--font-size-2);
  padding: var(--size-3) var(--size-7);
}
```

---

## Card

### card.tsx

```tsx
import styles from './card.module.css';
import { cx } from '@/lib/classnames';

interface CardProps extends React.HTMLAttributes<HTMLDivElement> {
  variant?: 'default' | 'interactive' | 'outlined';
  padding?: 'sm' | 'default' | 'lg';
}

export function Card({
  variant = 'default',
  padding = 'default',
  className,
  children,
  ...props
}: CardProps) {
  return (
    <div
      className={cx(
        styles.card,
        variant !== 'default' && styles[variant],
        padding !== 'default' && styles[`padding${padding.charAt(0).toUpperCase() + padding.slice(1)}`],
        className,
      )}
      {...props}
    >
      {children}
    </div>
  );
}
```

### card.module.css

```css
.card {
  background: var(--color-bg);
  border: var(--border-size-1) solid var(--color-border);
  border-radius: var(--radius-2);
  padding: var(--size-5);
  box-shadow: var(--shadow-card);
}

.interactive {
  cursor: pointer;
  transition: box-shadow 0.2s var(--ease-3), transform 0.2s var(--ease-3);

  &:hover {
    box-shadow: var(--shadow-card-hover);
    transform: translateY(-2px);
  }

  &:focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: 2px;
  }
}

.outlined {
  box-shadow: none;
}

.paddingSm { padding: var(--size-3); }
.paddingLg { padding: var(--size-7); }
```

---

## Input

### input.tsx

```tsx
import styles from './input.module.css';
import { cx } from '@/lib/classnames';

interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label: string;
  error?: string;
  hint?: string;
}

export function Input({
  label,
  error,
  hint,
  id,
  className,
  ...props
}: InputProps) {
  const inputId = id || label.toLowerCase().replace(/\s+/g, '-');

  return (
    <div className={cx(styles.field, className)}>
      <label htmlFor={inputId} className={styles.label}>
        {label}
      </label>
      <input
        id={inputId}
        className={cx(styles.input, error && styles.error)}
        aria-invalid={!!error}
        aria-describedby={error ? `${inputId}-error` : hint ? `${inputId}-hint` : undefined}
        {...props}
      />
      {error && (
        <p id={`${inputId}-error`} className={styles.errorText} role="alert">
          {error}
        </p>
      )}
      {hint && !error && (
        <p id={`${inputId}-hint`} className={styles.hint}>
          {hint}
        </p>
      )}
    </div>
  );
}
```

### input.module.css

```css
.field {
  display: flex;
  flex-direction: column;
  gap: var(--size-1);
}

.label {
  font-size: var(--font-size-0);
  font-weight: var(--font-weight-5);
  color: var(--color-text-1);
}

.input {
  font-family: inherit;
  font-size: var(--font-size-1);
  padding: var(--size-2) var(--size-3);
  border: var(--border-size-1) solid var(--color-border);
  border-radius: var(--radius-2);
  background: var(--color-bg);
  color: var(--color-text-1);
  transition: border-color 0.15s var(--ease-3), box-shadow 0.15s var(--ease-3);
  width: 100%;

  &::placeholder {
    color: var(--color-text-3);
  }

  &:hover {
    border-color: var(--color-text-3);
  }

  &:focus {
    outline: none;
    border-color: var(--color-primary);
    box-shadow: 0 0 0 3px var(--color-primary-subtle);
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}

.error {
  border-color: var(--color-destructive);

  &:focus {
    border-color: var(--color-destructive);
    box-shadow: 0 0 0 3px var(--color-destructive-subtle);
  }
}

.errorText {
  font-size: var(--font-size-0);
  color: var(--color-destructive);
}

.hint {
  font-size: var(--font-size-0);
  color: var(--color-text-3);
}
```

---

## Textarea

### textarea.tsx

```tsx
import styles from './textarea.module.css';
import { cx } from '@/lib/classnames';

interface TextareaProps extends React.TextareaHTMLAttributes<HTMLTextAreaElement> {
  label: string;
  error?: string;
}

export function Textarea({ label, error, id, className, ...props }: TextareaProps) {
  const textareaId = id || label.toLowerCase().replace(/\s+/g, '-');

  return (
    <div className={cx(styles.field, className)}>
      <label htmlFor={textareaId} className={styles.label}>{label}</label>
      <textarea
        id={textareaId}
        className={cx(styles.textarea, error && styles.error)}
        aria-invalid={!!error}
        {...props}
      />
      {error && <p className={styles.errorText} role="alert">{error}</p>}
    </div>
  );
}
```

### textarea.module.css

```css
.field {
  display: flex;
  flex-direction: column;
  gap: var(--size-1);
}

.label {
  font-size: var(--font-size-0);
  font-weight: var(--font-weight-5);
  color: var(--color-text-1);
}

.textarea {
  font-family: inherit;
  font-size: var(--font-size-1);
  padding: var(--size-2) var(--size-3);
  border: var(--border-size-1) solid var(--color-border);
  border-radius: var(--radius-2);
  background: var(--color-bg);
  color: var(--color-text-1);
  transition: border-color 0.15s var(--ease-3);
  width: 100%;
  min-height: 120px;
  resize: vertical;

  &:focus {
    outline: none;
    border-color: var(--color-primary);
    box-shadow: 0 0 0 3px var(--color-primary-subtle);
  }
}

.error {
  border-color: var(--color-destructive);
}

.errorText {
  font-size: var(--font-size-0);
  color: var(--color-destructive);
}
```

---

## Badge

### badge.tsx

```tsx
import styles from './badge.module.css';
import { cx } from '@/lib/classnames';

interface BadgeProps extends React.HTMLAttributes<HTMLSpanElement> {
  variant?: 'default' | 'success' | 'warning' | 'destructive' | 'info';
}

export function Badge({ variant = 'default', className, children, ...props }: BadgeProps) {
  return (
    <span className={cx(styles.badge, styles[variant], className)} {...props}>
      {children}
    </span>
  );
}
```

### badge.module.css

```css
.badge {
  display: inline-flex;
  align-items: center;
  font-size: var(--font-size-00);
  font-weight: var(--font-weight-6);
  padding: var(--size-1) var(--size-2);
  border-radius: var(--radius-round);
  white-space: nowrap;
}

.default {
  background: var(--color-surface-2);
  color: var(--color-text-2);
}

.success {
  background: var(--color-success-subtle);
  color: var(--color-success);
}

.warning {
  background: var(--color-warning-subtle);
  color: var(--color-warning);
}

.destructive {
  background: var(--color-destructive-subtle);
  color: var(--color-destructive);
}

.info {
  background: var(--color-info-subtle);
  color: var(--color-info);
}
```

---

## Modal / Dialog

This is a client component because it manages open/close state and focus trapping.

### modal.tsx

```tsx
'use client';

import { useEffect, useRef } from 'react';
import styles from './modal.module.css';
import { cx } from '@/lib/classnames';

interface ModalProps {
  open: boolean;
  onClose: () => void;
  title: string;
  children: React.ReactNode;
  className?: string;
}

export function Modal({ open, onClose, title, children, className }: ModalProps) {
  const dialogRef = useRef<HTMLDialogElement>(null);

  useEffect(() => {
    const dialog = dialogRef.current;
    if (!dialog) return;

    if (open) {
      dialog.showModal();
    } else {
      dialog.close();
    }
  }, [open]);

  useEffect(() => {
    const dialog = dialogRef.current;
    if (!dialog) return;

    const handleClose = () => onClose();
    dialog.addEventListener('close', handleClose);
    return () => dialog.removeEventListener('close', handleClose);
  }, [onClose]);

  return (
    <dialog ref={dialogRef} className={cx(styles.modal, className)}>
      <div className={styles.header}>
        <h2 className={styles.title}>{title}</h2>
        <button className={styles.closeButton} onClick={onClose} aria-label="Close">
          ✕
        </button>
      </div>
      <div className={styles.content}>
        {children}
      </div>
    </dialog>
  );
}
```

### modal.module.css

```css
.modal {
  border: none;
  border-radius: var(--radius-3);
  padding: 0;
  max-width: min(560px, 90vw);
  width: 100%;
  background: var(--color-bg);
  box-shadow: var(--shadow-modal);
  color: var(--color-text-2);

  &::backdrop {
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(4px);
  }

  &[open] {
    animation: slideUp 0.2s var(--ease-3);
  }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(var(--size-3));
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--size-4) var(--size-5);
  border-bottom: var(--border-size-1) solid var(--color-border);
}

.title {
  font-size: var(--font-size-2);
  font-weight: var(--font-weight-6);
  color: var(--color-text-1);
}

.closeButton {
  background: none;
  border: none;
  cursor: pointer;
  font-size: var(--font-size-2);
  color: var(--color-text-3);
  padding: var(--size-1);
  border-radius: var(--radius-2);
  transition: background 0.15s var(--ease-3);

  &:hover { background: var(--color-surface-1); }
  &:focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: 2px;
  }
}

.content {
  padding: var(--size-5);
}

@media (prefers-reduced-motion: reduce) {
  .modal[open] {
    animation: none;
  }
}
```

---

## Accordion

### accordion.tsx

```tsx
'use client';

import { useState } from 'react';
import styles from './accordion.module.css';
import { cx } from '@/lib/classnames';

interface AccordionItem {
  id: string;
  title: string;
  content: React.ReactNode;
}

interface AccordionProps {
  items: AccordionItem[];
  allowMultiple?: boolean;
  className?: string;
}

export function Accordion({ items, allowMultiple = false, className }: AccordionProps) {
  const [openIds, setOpenIds] = useState<Set<string>>(new Set());

  const toggle = (id: string) => {
    setOpenIds((prev) => {
      const next = new Set(allowMultiple ? prev : []);
      if (prev.has(id)) {
        next.delete(id);
      } else {
        next.add(id);
      }
      return next;
    });
  };

  return (
    <div className={cx(styles.accordion, className)}>
      {items.map((item) => {
        const isOpen = openIds.has(item.id);
        return (
          <div key={item.id} className={styles.item}>
            <button
              className={styles.trigger}
              onClick={() => toggle(item.id)}
              aria-expanded={isOpen}
              aria-controls={`accordion-panel-${item.id}`}
            >
              <span className={styles.triggerText}>{item.title}</span>
              <span className={cx(styles.icon, isOpen && styles.iconOpen)}>▸</span>
            </button>
            <div
              id={`accordion-panel-${item.id}`}
              role="region"
              className={cx(styles.panel, isOpen && styles.panelOpen)}
              hidden={!isOpen}
            >
              <div className={styles.panelContent}>
                {item.content}
              </div>
            </div>
          </div>
        );
      })}
    </div>
  );
}
```

### accordion.module.css

```css
.accordion {
  border: var(--border-size-1) solid var(--color-border);
  border-radius: var(--radius-2);
  overflow: hidden;
}

.item + .item {
  border-top: var(--border-size-1) solid var(--color-border);
}

.trigger {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: var(--size-3) var(--size-4);
  background: none;
  border: none;
  cursor: pointer;
  font-family: inherit;
  font-size: var(--font-size-1);
  font-weight: var(--font-weight-5);
  color: var(--color-text-1);
  text-align: left;
  transition: background 0.15s var(--ease-3);

  &:hover { background: var(--color-surface-1); }
  &:focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: -2px;
  }
}

.icon {
  font-size: var(--font-size-2);
  color: var(--color-text-3);
  transition: transform 0.2s var(--ease-3);
}

.iconOpen {
  transform: rotate(90deg);
}

.panel {
  overflow: hidden;
}

.panelContent {
  padding: 0 var(--size-4) var(--size-4);
  color: var(--color-text-2);
  font-size: var(--font-size-1);
}
```

---

## Navigation Header

### header.tsx

```tsx
import Link from 'next/link';
import styles from './header.module.css';
import { NAV_LINKS } from '@/lib/constants';

export function Header() {
  return (
    <header className={styles.header}>
      <div className={styles.container}>
        <Link href="/" className={styles.logo}>
          {/* Replace with your logo component */}
          <span className={styles.logoText}>[Logo]</span>
        </Link>

        <nav className={styles.nav} aria-label="Main navigation">
          {NAV_LINKS.map((link) => (
            <Link key={link.href} href={link.href} className={styles.navLink}>
              {link.label}
            </Link>
          ))}
        </nav>

        <div className={styles.actions}>
          {/* CTA button, theme toggle, etc. */}
        </div>
      </div>
    </header>
  );
}
```

### header.module.css

```css
.header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: var(--color-bg);
  border-bottom: var(--border-size-1) solid var(--color-border);
  backdrop-filter: blur(12px);
}

.container {
  max-width: min(1280px, 100% - var(--size-8));
  margin-inline: auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--size-3) 0;
}

.logo {
  text-decoration: none;
  display: flex;
  align-items: center;
}

.logoText {
  font-size: var(--font-size-2);
  font-weight: var(--font-weight-7);
  color: var(--color-text-1);
}

.nav {
  display: none;
  align-items: center;
  gap: var(--size-1);
}

@media (min-width: 768px) {
  .nav { display: flex; }
}

.navLink {
  font-size: var(--font-size-0);
  font-weight: var(--font-weight-5);
  color: var(--color-text-2);
  padding: var(--size-1) var(--size-3);
  border-radius: var(--radius-2);
  transition: color 0.15s var(--ease-3), background 0.15s var(--ease-3);

  &:hover {
    color: var(--color-text-1);
    background: var(--color-surface-1);
  }
}

.actions {
  display: flex;
  align-items: center;
  gap: var(--size-2);
}
```

---

## Mobile Navigation

A slide-out mobile menu. Client component for open/close state.

### mobile-nav.tsx

```tsx
'use client';

import { useState } from 'react';
import Link from 'next/link';
import styles from './mobile-nav.module.css';
import { cx } from '@/lib/classnames';
import { NAV_LINKS } from '@/lib/constants';

export function MobileNav() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div className={styles.mobileNav}>
      <button
        className={styles.menuButton}
        onClick={() => setIsOpen(!isOpen)}
        aria-expanded={isOpen}
        aria-label={isOpen ? 'Close menu' : 'Open menu'}
      >
        <span className={cx(styles.bar, isOpen && styles.barOpen)} />
        <span className={cx(styles.bar, isOpen && styles.barOpen)} />
        <span className={cx(styles.bar, isOpen && styles.barOpen)} />
      </button>

      {isOpen && (
        <div className={styles.overlay} onClick={() => setIsOpen(false)}>
          <nav
            className={styles.panel}
            onClick={(e) => e.stopPropagation()}
            aria-label="Mobile navigation"
          >
            {NAV_LINKS.map((link) => (
              <Link
                key={link.href}
                href={link.href}
                className={styles.link}
                onClick={() => setIsOpen(false)}
              >
                {link.label}
              </Link>
            ))}
          </nav>
        </div>
      )}
    </div>
  );
}
```

### mobile-nav.module.css

```css
.mobileNav {
  display: block;
}

@media (min-width: 768px) {
  .mobileNav { display: none; }
}

.menuButton {
  display: flex;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: var(--size-1);
}

.bar {
  display: block;
  width: 24px;
  height: 2px;
  background: var(--color-text-1);
  transition: transform 0.2s var(--ease-3), opacity 0.2s var(--ease-3);
}

.barOpen:nth-child(1) { transform: translateY(7px) rotate(45deg); }
.barOpen:nth-child(2) { opacity: 0; }
.barOpen:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
  z-index: 200;
  animation: fadeIn 0.15s var(--ease-3);
}

.panel {
  position: absolute;
  top: 0;
  right: 0;
  width: min(320px, 85vw);
  height: 100%;
  background: var(--color-bg);
  padding: var(--size-8) var(--size-5);
  display: flex;
  flex-direction: column;
  gap: var(--size-1);
  box-shadow: var(--shadow-modal);
  animation: slideIn 0.2s var(--ease-3);
}

.link {
  font-size: var(--font-size-2);
  font-weight: var(--font-weight-5);
  color: var(--color-text-2);
  padding: var(--size-2) var(--size-3);
  border-radius: var(--radius-2);
  text-decoration: none;
  transition: background 0.15s var(--ease-3);

  &:hover {
    background: var(--color-surface-1);
    color: var(--color-text-1);
  }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideIn {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

@media (prefers-reduced-motion: reduce) {
  .overlay, .panel {
    animation: none;
  }
}
```

---

## Footer

### footer.tsx

```tsx
import Link from 'next/link';
import styles from './footer.module.css';

interface FooterColumn {
  title: string;
  links: { label: string; href: string }[];
}

interface FooterProps {
  columns: FooterColumn[];
  companyName: string;
}

export function Footer({ columns, companyName }: FooterProps) {
  const year = new Date().getFullYear();

  return (
    <footer className={styles.footer}>
      <div className={styles.container}>
        <div className={styles.grid}>
          {columns.map((col) => (
            <div key={col.title} className={styles.column}>
              <h3 className={styles.columnTitle}>{col.title}</h3>
              <ul className={styles.linkList}>
                {col.links.map((link) => (
                  <li key={link.href}>
                    <Link href={link.href} className={styles.link}>
                      {link.label}
                    </Link>
                  </li>
                ))}
              </ul>
            </div>
          ))}
        </div>

        <div className={styles.bottom}>
          <p className={styles.copyright}>
            © {year} {companyName}. All rights reserved.
          </p>
        </div>
      </div>
    </footer>
  );
}
```

### footer.module.css

```css
.footer {
  border-top: var(--border-size-1) solid var(--color-border);
  background: var(--color-surface-1);
  padding: var(--size-10) var(--size-4);
}

.container {
  max-width: min(1280px, 100% - var(--size-8));
  margin-inline: auto;
}

.grid {
  display: grid;
  gap: var(--size-6);
  grid-template-columns: repeat(2, 1fr);
}

@media (min-width: 768px) {
  .grid { grid-template-columns: repeat(4, 1fr); }
}

.columnTitle {
  font-size: var(--font-size-0);
  font-weight: var(--font-weight-6);
  color: var(--color-text-1);
  margin-bottom: var(--size-3);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.linkList {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: var(--size-2);
}

.link {
  font-size: var(--font-size-0);
  color: var(--color-text-3);
  text-decoration: none;
  transition: color 0.15s var(--ease-3);

  &:hover { color: var(--color-text-1); }
}

.bottom {
  margin-top: var(--size-8);
  padding-top: var(--size-5);
  border-top: var(--border-size-1) solid var(--color-border);
}

.copyright {
  font-size: var(--font-size-00);
  color: var(--color-text-3);
}
```

---

## Theme Toggle

Client component for switching between light and dark mode.

### theme-toggle.tsx

```tsx
'use client';

import { useEffect, useState } from 'react';
import styles from './theme-toggle.module.css';

export function ThemeToggle() {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');

  useEffect(() => {
    const stored = localStorage.getItem('theme');
    const preferred = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
    const initial = (stored as 'light' | 'dark') || preferred;
    setTheme(initial);
    document.documentElement.setAttribute('data-theme', initial);
  }, []);

  const toggle = () => {
    const next = theme === 'light' ? 'dark' : 'light';
    setTheme(next);
    document.documentElement.setAttribute('data-theme', next);
    localStorage.setItem('theme', next);
  };

  return (
    <button
      className={styles.toggle}
      onClick={toggle}
      aria-label={`Switch to ${theme === 'light' ? 'dark' : 'light'} mode`}
    >
      <span className={styles.icon} aria-hidden="true">
        {theme === 'light' ? '🌙' : '☀️'}
      </span>
    </button>
  );
}
```

### theme-toggle.module.css

```css
.toggle {
  background: none;
  border: var(--border-size-1) solid var(--color-border);
  border-radius: var(--radius-round);
  cursor: pointer;
  padding: var(--size-1);
  display: flex;
  align-items: center;
  justify-content: center;
  width: var(--size-7);
  height: var(--size-7);
  transition: background 0.15s var(--ease-3);

  &:hover {
    background: var(--color-surface-1);
  }

  &:focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: 2px;
  }
}

.icon {
  font-size: var(--font-size-1);
  line-height: 1;
}
```
