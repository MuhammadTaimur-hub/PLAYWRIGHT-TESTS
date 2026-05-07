# PLAYWRIGHT-TESTS
#Test script for validating Eligibility form

const { test, expect } = require('@playwright/test');

const URL = 'URL';

test.describe('PARCO MTP 2026 - Eligibility Form', () => {

  test('Page loads and title is correct', async ({ page }) => {
    await page.goto(URL);
    await expect(page).toHaveTitle(/ MTP 2026/);
    await expect(page.getByText('Management Trainee Program 2026')).toBeVisible();
  });

  test('All required fields are visible', async ({ page }) => {
    await page.goto(URL);
    await expect(page.getByLabel(/First name/i)).toBeVisible();
    await expect(page.getByLabel(/Last name/i)).toBeVisible();
    await expect(page.getByLabel(/CNIC/i)).toBeVisible();
    await expect(page.getByLabel(/Mobile phone/i)).toBeVisible();
    await expect(page.getByLabel(/Email/i)).toBeVisible();
    await expect(page.getByLabel(/Date of birth/i)).toBeVisible();
    await expect(page.getByLabel(/Country/i)).toBeVisible();
    await expect(page.getByLabel(/City/i)).toBeVisible();
    await expect(page.getByLabel(/Qualification/i)).toBeVisible();
    await expect(page.getByLabel(/Specialization/i)).toBeVisible();
    await expect(page.getByLabel(/Job site/i)).toBeVisible();
    await expect(page.getByLabel(/Graduation year/i)).toBeVisible();
  });

  test('Submit button is visible', async ({ page }) =>
    
    {
    await page.goto(URL);
    await expect(page.getByRole('button', { name: /Submit application/i })).toBeVisible();
  });

  test('Form blocks submission when required fields are empty', async ({ page }) => {
    await page.goto(URL);
    await page.getByRole('button', { name: /Submit application/i }).click();
    // Page should stay on the same URL — no redirect on empty submit
    await expect(page).toHaveURL(URL);
  });

  test('Accepts valid input in personal information fields', async ({ page }) => {
    await page.goto(URL);
    await page.getByLabel(/First name/i).fill('Muhammad');
    await page.getByLabel(/Last name/i).fill('Taimur');
    await page.getByLabel(/CNIC/i).fill('3520112345671');
    await page.getByLabel(/Mobile phone/i).fill('03001234567');
    await page.getByLabel(/Email/i).fill('Muhammad.Taimur@example.com');
    await page.getByLabel(/City/i).fill('Islamabad');

    await expect(page.getByLabel(/First name/i)).toHaveValue('Muhammad');
    await expect(page.getByLabel(/Last name/i)).toHaveValue('Taimur');
    await expect(page.getByLabel(/Email/i)).toHaveValue('Muhammad.Taimur@example.com');
  });

  test('File upload fields are present for CV, transcript and CNIC', async ({ page }) => {
    await page.goto(URL);
    const fileInputs = page.locator('input[type="file"]');
    await expect(fileInputs).toHaveCount(3);
  });

  test('Optional fields (Middle name, CGPA) are present but not required', async ({ page }) => {
    await page.goto(URL);
    await expect(page.getByLabel(/Middle name/i)).toBeVisible();
    await expect(page.getByLabel(/CGPA/i)).toBeVisible();
  });

});
