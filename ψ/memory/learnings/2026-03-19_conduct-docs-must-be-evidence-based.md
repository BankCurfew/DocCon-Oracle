# Conduct Documents Must Be Evidence-Based

**Date**: 2026-03-19
**Source**: Session 3 — Creating Jarvis Bot Conduct + FA Tools Code Conduct

## Pattern

เมื่อสร้าง conduct document (quality standard) ทุก rule และทุก violation ต้องมี evidence ชี้ได้ — thread number, file:line, test score, ข้อมูลจาก source จริง. ห้ามเป็น opinion.

## ทำไม

- Jarvis Conduct: ทุก winning technique มาจาก Researcher 10,956 msgs analysis, ทุก bug มาจาก QA Round 1 50% PASS, ทุก edge case มาจาก AIA domain review
- FA Tools Code Conduct: ทุก violation ชี้ file + line + evidence (เช่น `strict: false` ใน tsconfig.json, 20+ `any` ใน ApplicationForm.tsx:71-136)
- ถ้าไม่มี evidence → oracle ที่ถูกชี้จะ push back ว่า "ไม่จริง" → conduct ไม่มีน้ำหนัก

## วิธีใช้

1. ก่อนเขียน rule → หา source (thread, file, data)
2. ก่อน flag violation → ต้องอ้าง evidence ได้
3. Overall score → คำนวณจาก sub-scores ไม่ใช่ feeling
4. "Patterns Over Intentions" — Principle #2 ของ DocCon
