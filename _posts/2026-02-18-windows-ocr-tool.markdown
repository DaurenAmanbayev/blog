---
layout:     post
title:      "Building a Native OCR Tool with .NET 8"
subtitle:   "Leveraging Windows.Media.Ocr for high-performance local text extraction"
date:       2026-02-18 12:00:00
author:     "Dauren Amanbayev"
header-img: "img/post-bg-01.jpg"
tags:       [C#, .NET, OCR, Open Source]
---

<p>Cloud-based OCR solutions are powerful, but sometimes you need a tool that runs locally—securely, without API costs, and with zero latency. I decided to build exactly that using the power of the Windows Runtime (WinRT) APIs.</p>

<h2 class="section-heading">What is WindowsImagePdfOcr?</h2>

<p><strong>WindowsImagePdfOcr</strong> is a command-line tool and library implemented in <strong>C# targeting .NET 8</strong>. It acts as a wrapper around the built-in <code>Windows.Media.Ocr</code> engine, allowing developers and power users to extract text from images and PDF documents directly on their desktop environment.</p>

<p>The project focuses on accuracy and ease of use, automating the tedious process of converting scanned documents into editable <code>.txt</code> files.</p>

<h2 class="section-heading">Key Features</h2>

<p>Unlike simple wrappers, this tool handles the entire pipeline of image processing to ensure the best recognition results:</p>

<ul>
    <li><strong>PDF Support:</strong> Automatically renders PDF documents page-by-page into high-resolution images for recognition.</li>
    <li><strong>Smart Preprocessing:</strong> The engine doesn't just read the image; it improves it. It applies <em>padding</em>, <em>color inversion</em>, and <em>uniform scaling</em> to help the OCR engine detect characters more accurately.</li>
    <li><strong>Format Agnostic:</strong> Works out-of-the-box with PNG, JPEG, BMP, TIFF, and GIF.</li>
    <li><strong>Multi-Language Support:</strong> Includes a reusable OCR engine wrapper that supports language selection (e.g., <code>ru-RU</code>, <code>en-US</code>) based on installed Windows language packs.</li>
    <li><strong>WinRT Integration:</strong> Designed to run on Windows environments that support native WinRT OCR APIs.</li>
</ul>

<h2 class="section-heading">Technical Implementation</h2>

<p>The core challenge was bridging .NET 8 with Windows native APIs to handle visual data efficiently. By utilizing the <code>Windows.Media.Ocr</code> namespace, the tool achieves enterprise-grade recognition quality without external dependencies like Tesseract.</p>

<p>Extracted text is automatically saved to a <code>.txt</code> file located next to the input source, making batch processing of archives seamless.</p>

<blockquote>
  "The best tool is the one that respects your data privacy and works offline."
</blockquote>

<p>You can explore the source code, check the preprocessing logic, or download the binary from the repository below.</p>

<a href="https://github.com/DaurenAmanbayev/WindowsImagePdfOcr"><strong>WindowsImagePdfOcr</strong></a>