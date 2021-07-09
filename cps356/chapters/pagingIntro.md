# Paging Introduction

segmentation is breaking things (code, stack, heap) ito variable sized pieces,
however paging is breaking things into fixed sized piecesi (pages). the memory
is a fixed sized array that contains slots called page frames.

*crux: how to virtualize memory wiht pages, to avoid segmentation cons, what
are the basic techniques and how we make them work well? esp. with minimal space and time overheads*

# Simple Example and Overview

