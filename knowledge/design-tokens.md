# Design Tokens — Glassmorphism Dark Theme

> Source of truth for all visual values. Coder agent reads this before writing any UI.

## Colors

```typescript
export const Colors = {
  // Brand
  primary:    '#00E5FF',   // Cyan — primary actions, active states
  secondary:  '#7C3AED',   // Violet — secondary UI, territory type B
  accent1:    '#FF6B6B',   // Coral — warnings, contested territories
  accent2:    '#00C896',   // Mint — success, friendly territories

  // Background
  background:     '#080B14',  // Base dark
  surface:        '#0D1117',  // Cards, sheets
  surfaceElevated:'#161B22',  // Modals, drawers

  // Glass layers
  glass1: 'rgba(255, 255, 255, 0.03)',
  glass2: 'rgba(255, 255, 255, 0.06)',
  glass3: 'rgba(255, 255, 255, 0.10)',

  // Text
  textPrimary:   '#FFFFFF',
  textSecondary: 'rgba(255, 255, 255, 0.60)',
  textTertiary:  'rgba(255, 255, 255, 0.30)',

  // Borders
  borderSubtle:  'rgba(255, 255, 255, 0.08)',
  borderMedium:  'rgba(255, 255, 255, 0.15)',
  borderStrong:  'rgba(0, 229, 255, 0.30)',
};
```

## Typography

```typescript
export const Typography = {
  // Display — SF Pro Display (iOS) / Roboto (Android)
  displayLarge:  { fontSize: 34, fontWeight: '700', letterSpacing: -0.5 },
  displayMedium: { fontSize: 28, fontWeight: '600', letterSpacing: -0.3 },

  // Body
  bodyLarge:  { fontSize: 17, fontWeight: '400', lineHeight: 24 },
  bodyMedium: { fontSize: 15, fontWeight: '400', lineHeight: 22 },
  bodySmall:  { fontSize: 13, fontWeight: '400', lineHeight: 18 },

  // Label
  labelLarge:  { fontSize: 15, fontWeight: '600', letterSpacing: 0.1 },
  labelMedium: { fontSize: 13, fontWeight: '500', letterSpacing: 0.1 },

  // Mono — JetBrains Mono
  mono: { fontFamily: 'JetBrainsMono-Regular', fontSize: 13 },
};
```

## Spacing

```typescript
export const Spacing = {
  xs:  4,
  sm:  8,
  md:  16,
  lg:  24,
  xl:  32,
  xxl: 48,
};
```

## Border Radius

```typescript
export const Radius = {
  sm:   8,
  md:   16,
  lg:   24,
  xl:   32,
  pill: 999,
};
```

## Glass Component Recipe

```typescript
// Standard glass card
{
  backgroundColor: Colors.glass2,
  borderWidth: 1,
  borderColor: Colors.borderSubtle,
  borderRadius: Radius.lg,
  // iOS blur
  // Use @react-native-community/blur: <BlurView blurType="dark" blurAmount={20} />
}

// Active / focused glass card
{
  backgroundColor: Colors.glass3,
  borderColor: Colors.borderStrong,  // cyan border glow
}
```

## Animation Springs

```typescript
// Standard UI interaction
{ damping: 15, stiffness: 120 }

// Snappy button press
{ damping: 20, stiffness: 200 }

// Slow entrance
{ damping: 25, stiffness: 80 }
```
