# About Me 👋
Hi, I'm Young Su, a researcher working at the intersection of AI and biology. More specifically, I'm using **AI to better understand the behavior of proteins, such as how they interact with other proteins**. More broadly, my research spans protein language modeling, generative modeling, antibody engineering, and protein design. 

I'd love to connect if you have similar interests (`y4ko at ucsd dot edu`)! 

See [CV here](https://young-su-ko.github.io/assets/files/cv.pdf) | See summary of research and projects below ⬇️.

# Research

## forge: sequence-based binder design with latent flow matching (WIP)
Forge re-imagines de novo protein design from an AI-first perspective. Many protein design methods, such as RFDiffusion, aim to design proteins with specific functions by designing their structure. Given the sequence-structure-function paradigm, this makes sense, and has shown great success over the years. Models like RFDiffusion, trained on the PDB, only see a small subset of the entire protein functional space, as the PDB is largely limited to crystallizable, structured proteins. Thus, an unanswered question is how we can design binders for proteins underrepresented in the training data. 

Based on our previous research only using protein sequence information, we saw this as an opportunity for sequence-based binder design. Rather than relying on PDB complex structures to learn how proteins interact, could we leverage the much larger STRING PPI database? Like text-to-image generation models, we could train a generative model that takes as input a target protein (analgous to text) and returns a binder protein (analgous to image). Instead of using the raw amino acid sequences, we can use a continuous latent representation to capture additional semantic information, naturally motivating latent flow matching. Rather than training our own protein encoder-decoders, we leverage the pre-trained [Raygun]() model. 

In short, **forge** does conditional generation of binder latents that get decoded back into sequence by Raygun. So far, we trained a 500M parameter latent flow matching model trained on ~10M high-confidence (>80%) STRING interactions for ~500k optimization steps. Currently, we are exploring the effects of increasing training time, adding length conditioning, and expanding our in-silico evaluation protocol. Eventually, we hope to highlight forge and the potential of sequence-based design in the wet-lab.

[Code](https://github.com/young-su-ko/forge-v0) | [Paper](https://www.mlsb.io/papers_2025/75.pdf) 
> Note: Sometimes Figure 1 does not render correctly on the Safari browser. Please try FireFox or Chrome in this case.

## TUnA: uncertainty-aware Transformer for protein-protein interaction prediction
While designing novel protein interactions with generative models is incredibly powerful, predicting interactions before they hit the wet-lab is another side of the protein design coin. TUnA is a sequence-based protein-protein interaction prediction model that takes in two protein sequences as input and returns a prediction as well as its confidence. Under the hood, we use ESM2 embeddings to represent protein sequences, a transformer backbone to learn pairwise interaction patterns, and spectral normalization + last layer Gaussian process to make uncertainty-aware predictions.

When we benchmarked TUnA against previous state-of-the-art models, we focused on the [Bernett dataset](https://academic.oup.com/bib/article/25/2/bbae076/7621029), a gold-standard dataset used to assess model generalizability to unseen proteins and PPIs. While many previous models achived near perfect accuracy on datasets without sequence similarity-based train/test splits, they all struggled to predict PPIs when trained on a more rigorous split. TUnA achieved an AUROC of 0.70 on the Bernett split, a 10 percentage point improvement from the previous best reported 0.60. 

Overall, this project taught me the importance of aligning AI tools with the needs of users. By introducing uncertainty-awareness to PPI prediction, we aimed to reduce wasted effort by experimentalists, allowing them to prioritize the experiments that are most likely to succeed.

[Code](https://github.com/young-su-ko/TUnA-R) | [Paper](https://academic.oup.com/bib/article/25/5/bbae359/7720609)

## Benchmarking text-integrated protein language models and embedding fusion

Protein language model embeddings are incredibly powerful representations that capture multiple aspects of a protein. Seeing an increasing number of studies incorporating CLIP-style training to align pLM representations with text-representations of protein function, I set out to benchmark these representations on classical protein engineering benchmarks. In doing so, I discovered no pLM, regardless of text-integration, always outperforms all embeddings. I then explored the potential of further enhancing protein representations with embedding fusion and proposed a greedy solution to perform embedding fusion at scale. 

[Code](https://github.com/Wang-lab-UCSD/Benchmarking-tpLMs) | [Paper](https://academic.oup.com/bib/article/27/1/bbag014/8444383)

## Using Raygun to design EGFR binders
As part of the [Adaptyv Bio EFGR binder design competition](https://www.adaptyvbio.com/blog/po104/), I designed an EGFR-binder with a 67% tighter binding affinity than EGF, the native ligand. I used a novel protein design pipeline using Raygun to generate longer EGF variants and ProTrek to perform sequence-based filtering. This experience showed me the potential of sequence-based protein design and inspired me to pursue sequence-based de novo design with forge.

[Code](https://github.com/rohitsinghlab/raygun) | [Paper](https://www.biorxiv.org/content/10.1101/2024.08.13.607858v2)

# Projects 

## Evaluating generated proteins with plm-fid

As I began to explore protein generative models, the most important question was how we can evaluate them. Unlike images, it's hard to eyeball whether a generated protein "looks right" and we often turn to computational methods as our oracles. One way to evaluate a protein generative model, first introduced by EvoDiff, is using the Frechet Distance of protein embeddings. Commonly used in image generation, the Frechet Inception Distance (FID) measured the Frechet distance between the Inception representations of generated images and the Inception representations of set of reference images. Smaller the distance means that the generated images are more like the reference distribution. While this distance is not without its limitations, it has become a standard way to assess protein generative models.

I created the `plm-fid` package to enable users to quickly compute the Frechet distance based on pLM embeddings. This packages allows users to pass in two sets of protein sequences (e.g. a FASTA file containing generated sequences and another FASTA file containing natural sequences from UniRef) and returns the Frechet distance when the sequences are embedded by a pLM. Since any pLM can be used, we offer the choice of multiple pLMs, including ESM2, ESM-C, ProtT5, etc. This was also my first time creating a Python package and uploading it to PyPI--it helped me see how engineering principles are important even as a research-oriented scientist.

[Code](https://github.com/young-su-ko/plm-fid) 

---

Last updated: February 2026 


