# Efficient-Video-Event-Search-Engine-Using-Multi-Modal-AI

# Project Overview

This project focuses on developing an efficient multi-modal video event search engine that allows users to search large video collections using natural-language queries.

The system combines visual, audio, and OCR-based textual information to identify relevant video moments. It introduces a Quality-Aware Adaptive Fusion (QAAF) approach that dynamically adjusts the contribution of each modality according to its quality.

The proposed system is training-free, requires no labeled data or model retraining, and is designed to provide frame-accurate video event retrieval with low latency.

# Research Objective

Develop a multi-modal video event search system
Search video content using natural-language queries
Combine visual, audio, and OCR information
Handle variations in modality quality
Improve temporal localization of relevant events
Reduce retrieval latency for large video collections
Develop a training-free adaptive fusion mechanism

# Business / Real-World Objective

The system can be applied to:

Video surveillance
Educational video archives
Sports video analysis
Enterprise video repositories
Social media video search
Personal video collections
Multimedia content management

The main objective is to allow users to locate specific events or moments in videos without manually watching the entire video collection.

# Project Workflow

1. Video Data Collection

The system processes real-world videos collected from different domains.

The evaluation corpus contains six YouTube videos across five domains:

Sports
Education
Personal Finance
Travel
Technology

The total corpus contains approximately 6,335.8 seconds of video, with 6,338 processed frames, 1,250 OCR frames, and 1,825 audio segments.

2. Video Preprocessing

The input videos are processed before feature extraction.

The preprocessing pipeline includes:

Video decoding using OpenCV
Frame extraction at 1 FPS
Frame resizing to 640 × 360
JPEG compression
Audio extraction using FFmpeg
Audio conversion to 16 kHz mono WAV
Handling videos without audio tracks

The system assigns zero audio weight to videos that do not contain an audio track.

3. Multi-Modal Feature Extraction

The system extracts information from three different modalities.

Visual Features

CLIP ViT-B/32 is used to extract visual representations from video frames.

512-dimensional feature vectors
L2 normalization
Batch processing
Cosine similarity for query matching

Audio Features

OpenAI Whisper Base is used for automatic speech recognition.

The system generates timestamped speech segments containing:

Start time
End time
Transcribed text

The audio relevance score combines query-word coverage and term frequency.

OCR Features

EasyOCR is used to detect and recognize text appearing on video frames.

OCR is performed approximately once every three seconds, while low-confidence detections and very short text are removed to reduce noise.

# Quality-Aware Adaptive Fusion

The core contribution of the project is Quality-Aware Adaptive Fusion (QAAF).

Traditional multi-modal systems often use fixed weights for audio, visual, and OCR features. QAAF dynamically adjusts these weights according to the quality of each modality for a particular video.

Quality is estimated using:

Audio: RMS energy distribution
Visual: Image sharpness and brightness
OCR: Detection confidence and OCR frame density

The adaptive weighting mechanism reduces the influence of poor-quality modalities and increases the contribution of reliable modalities.

# System Architecture

The overall system follows four major stages:

                Raw Video Input
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Visual        Audio         OCR
       Features      Features      Features
       CLIP          Whisper       EasyOCR
          │            │            │
          └────────────┼────────────┘
                       ▼
             Quality-Aware Adaptive
                  Fusion (QAAF)
                       │
                       ▼
              Temporal Window
                   Ranking
                       │
                       ▼
              Top-K Video Results

The paper's system architecture combines video preprocessing, modality-specific feature extraction, QAAF fusion, and ranked timestamp retrieval.

# Temporal Event Retrieval

To improve temporal localization, retrieval results are grouped into 10-second temporal windows.

For each window, the system calculates the strongest audio, visual, and OCR scores and combines them using the adaptive modality weights.

A multi-modal agreement bonus is also applied when multiple modalities identify relevant information within the same temporal window.

Finally, the system ranks all windows and returns the Top-K relevant results.

# Evaluation Metrics

The system is evaluated using standard information retrieval metrics:

Precision@5

Measures the proportion of relevant results among the top five retrieved results.

Recall@5

Measures how many of the relevant videos are retrieved within the top five results.

F1@5

Combines Precision@5 and Recall@5 into a single metric.

# Mean Reciprocal Rank (MRR)

Measures how highly the first relevant result appears in the ranked results.

The evaluation uses eight natural-language queries and applies a minimum retrieval threshold of 0.30.

# Results & Analysis

The proposed QAAF-based multi-modal system achieved:

Metric	Result
Precision@5	0.2667
Recall@5	0.5833
F1@5	0.3551
MRR	0.875

The complete multi-modal QAAF system achieved an F1@5 of 0.3551 and MRR of 0.875 on the evaluation corpus.

The ablation study also showed that Visual Only achieved an F1 score of 0.3720, while Audio Only and OCR Only produced an F1 score of 0.000. The proposed QAAF system achieved the highest F1 among the multi-modal configurations evaluated in the study.

# Key Findings

Adaptive Modality Weighting

QAAF dynamically changes the modality weights based on video quality.

For example:

Sports videos with heavy crowd noise received lower audio weights.
The English podcast received a higher audio weight because of clearer speech.
The AI/Technology video received a higher OCR weight because of dense on-screen text.

Retrieval Performance

The system achieved an MRR of 0.875, indicating strong ranking performance on the evaluated queries. The paper reports that the correct video appeared within the first two results for 87.5% of queries.

Low-Latency Retrieval

The proposed system reports an average retrieval latency of approximately 0.9 seconds and a 4.7× acceleration compared with the referenced common consumer system.

# Sample Search Queries

The system was evaluated using natural-language queries such as:

Goal scored football
Football match highlights
Player running on field
English learning podcast
Invest money teen
Monsoon rain village nature
Artificial intelligence
Machine learning beginners

These queries were mapped to manually verified relevant videos in the evaluation corpus.

# Tools & Technologies

OpenCV: Video decoding and frame extraction
FFmpeg: Audio extraction and preprocessing
CLIP ViT-B/32: Visual feature extraction
OpenAI Whisper: Audio transcription
EasyOCR: On-screen text extraction
Cosine Similarity: Visual query matching
Term Frequency Scoring: Audio and OCR relevance scoring
QAAF: Quality-aware multi-modal fusion
Temporal Ranking: Video event localization
GitHub: Source-code and project documentation

# Key Deliverables

Video preprocessing pipeline
Frame extraction module
Audio extraction module
CLIP-based visual feature extraction
Whisper-based audio transcription
EasyOCR-based text extraction
Quality-Aware Adaptive Fusion module
Temporal event ranking system
Natural-language video search
Top-K video retrieval
Retrieval evaluation using Precision, Recall, F1, and MRR
Research paper and documentation

# Project Structure

Efficient-Video-Event-Search/
│
├── data/
│   ├── videos/
│   ├── frames/
│   ├── audio/
│   └── ocr/
│
├── src/
│   ├── video_preprocessing.py
│   ├── visual_features.py
│   ├── audio_transcription.py
│   ├── ocr_extraction.py
│   ├── qaaf_fusion.py
│   ├── temporal_ranking.py
│   └── video_search.py
│
├── notebooks/
│   └── video_event_search.ipynb
│
├── results/
│   ├── retrieval_results/
│   ├── evaluation_metrics/
│   └── visualizations/
│
├── research-paper/
│   └── Efficient_Video_Event_Search_Engine.pdf
│
├── requirements.txt
├── README.md
└── LICENSE

# Research Contribution

The primary contribution of this work is the Quality-Aware Adaptive Fusion (QAAF) framework.

Instead of assigning fixed weights to audio, visual, and OCR modalities, the system estimates the quality of each modality independently and dynamically adjusts its contribution during retrieval.

This enables the system to adapt to different video conditions such as:

Noisy audio
Silent videos
Blurred frames
Overexposed frames
Dense on-screen text
Sparse OCR information

The approach is designed to improve the robustness of multi-modal video retrieval without requiring additional labeled training data or model retraining.

# Future Scope

The research identifies several directions for future development:

Neural-based quality prediction
Improved temporal reasoning
Before-and-after event understanding
Cross-scene action tracking
Domain-specific model tuning
Evaluation on larger benchmark datasets
Testing on MSRVTT
Testing on ActivityNet Captions
Further optimization of retrieval performance

# Research Paper

Title: Efficient Video Event Search Engine Using Multi-Modal AI

Core Contribution: Quality-Aware Adaptive Fusion (QAAF)

# Research Area:

Artificial Intelligence, Multi-Modal AI, Computer Vision, Natural Language Processing, Video Retrieval, Information Retrieval

The paper presents a training-free multi-modal video retrieval architecture combining visual, audio, and OCR signals for natural-language video event search.

# Authors

Sourov Kumar Nandi,
Md Shakil Hossen,
Avishek Golder,
Tarak Aggarwal,
Vanshika,
Puspa Raj Khadka,

Department of Computer Science & Engineering
Chandigarh University
Mohali, Punjab, India

Keywords

Multi-Modal AI, Video Retrieval, Video Event Search ,Computer Vision ,CLIP ,Whisper, EasyOCR, OCR ,Natural Language Processing, Information Retrieval,T emporal Localization, Quality-Aware Fusion, QAAF, Artificial Intelligence
