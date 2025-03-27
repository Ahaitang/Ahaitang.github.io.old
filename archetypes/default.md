+++
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
slug = '{{ replace .File.ContentBaseName "-" " " | slug }}'
date = {{ .Date }}
weight=5
draft = false
author={{ .Date }}
categories = [
]
+++