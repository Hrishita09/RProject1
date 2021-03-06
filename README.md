# RProject1
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "Heart Disease Predication using ml.ipynb",
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "lrWWaCwbNe5w",
        "colab_type": "text"
      },
      "source": [
        "# Import Packages\n",
        "\n",
        "```\n",
        "# This is formatted as code\n",
        "```\n",
        "\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "GPvoE9uSNh5z",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "import numpy as np\n",
        "import pandas as pd\n",
        "import matplotlib.pyplot as plt\n",
        "from matplotlib import rcParams\n",
        "from matplotlib.cm import rainbow\n",
        "%matplotlib inline\n",
        "import warnings\n",
        "warnings.filterwarnings('ignore')"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "p6xIRC-aJtup",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "from sklearn.neighbors import KNeighborsClassifier\n"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "TRis5dGAKaCj",
        "colab_type": "code",
        "colab": {
          "resources": {
            "http://localhost:8080/nbextensions/google.colab/files.js": {
              "data": "Ly8gQ29weXJpZ2h0IDIwMTcgR29vZ2xlIExMQwovLwovLyBMaWNlbnNlZCB1bmRlciB0aGUgQXBhY2hlIExpY2Vuc2UsIFZlcnNpb24gMi4wICh0aGUgIkxpY2Vuc2UiKTsKLy8geW91IG1heSBub3QgdXNlIHRoaXMgZmlsZSBleGNlcHQgaW4gY29tcGxpYW5jZSB3aXRoIHRoZSBMaWNlbnNlLgovLyBZb3UgbWF5IG9idGFpbiBhIGNvcHkgb2YgdGhlIExpY2Vuc2UgYXQKLy8KLy8gICAgICBodHRwOi8vd3d3LmFwYWNoZS5vcmcvbGljZW5zZXMvTElDRU5TRS0yLjAKLy8KLy8gVW5sZXNzIHJlcXVpcmVkIGJ5IGFwcGxpY2FibGUgbGF3IG9yIGFncmVlZCB0byBpbiB3cml0aW5nLCBzb2Z0d2FyZQovLyBkaXN0cmlidXRlZCB1bmRlciB0aGUgTGljZW5zZSBpcyBkaXN0cmlidXRlZCBvbiBhbiAiQVMgSVMiIEJBU0lTLAovLyBXSVRIT1VUIFdBUlJBTlRJRVMgT1IgQ09ORElUSU9OUyBPRiBBTlkgS0lORCwgZWl0aGVyIGV4cHJlc3Mgb3IgaW1wbGllZC4KLy8gU2VlIHRoZSBMaWNlbnNlIGZvciB0aGUgc3BlY2lmaWMgbGFuZ3VhZ2UgZ292ZXJuaW5nIHBlcm1pc3Npb25zIGFuZAovLyBsaW1pdGF0aW9ucyB1bmRlciB0aGUgTGljZW5zZS4KCi8qKgogKiBAZmlsZW92ZXJ2aWV3IEhlbHBlcnMgZm9yIGdvb2dsZS5jb2xhYiBQeXRob24gbW9kdWxlLgogKi8KKGZ1bmN0aW9uKHNjb3BlKSB7CmZ1bmN0aW9uIHNwYW4odGV4dCwgc3R5bGVBdHRyaWJ1dGVzID0ge30pIHsKICBjb25zdCBlbGVtZW50ID0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudCgnc3BhbicpOwogIGVsZW1lbnQudGV4dENvbnRlbnQgPSB0ZXh0OwogIGZvciAoY29uc3Qga2V5IG9mIE9iamVjdC5rZXlzKHN0eWxlQXR0cmlidXRlcykpIHsKICAgIGVsZW1lbnQuc3R5bGVba2V5XSA9IHN0eWxlQXR0cmlidXRlc1trZXldOwogIH0KICByZXR1cm4gZWxlbWVudDsKfQoKLy8gTWF4IG51bWJlciBvZiBieXRlcyB3aGljaCB3aWxsIGJlIHVwbG9hZGVkIGF0IGEgdGltZS4KY29uc3QgTUFYX1BBWUxPQURfU0laRSA9IDEwMCAqIDEwMjQ7Ci8vIE1heCBhbW91bnQgb2YgdGltZSB0byBibG9jayB3YWl0aW5nIGZvciB0aGUgdXNlci4KY29uc3QgRklMRV9DSEFOR0VfVElNRU9VVF9NUyA9IDMwICogMTAwMDsKCmZ1bmN0aW9uIF91cGxvYWRGaWxlcyhpbnB1dElkLCBvdXRwdXRJZCkgewogIGNvbnN0IHN0ZXBzID0gdXBsb2FkRmlsZXNTdGVwKGlucHV0SWQsIG91dHB1dElkKTsKICBjb25zdCBvdXRwdXRFbGVtZW50ID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQob3V0cHV0SWQpOwogIC8vIENhY2hlIHN0ZXBzIG9uIHRoZSBvdXRwdXRFbGVtZW50IHRvIG1ha2UgaXQgYXZhaWxhYmxlIGZvciB0aGUgbmV4dCBjYWxsCiAgLy8gdG8gdXBsb2FkRmlsZXNDb250aW51ZSBmcm9tIFB5dGhvbi4KICBvdXRwdXRFbGVtZW50LnN0ZXBzID0gc3RlcHM7CgogIHJldHVybiBfdXBsb2FkRmlsZXNDb250aW51ZShvdXRwdXRJZCk7Cn0KCi8vIFRoaXMgaXMgcm91Z2hseSBhbiBhc3luYyBnZW5lcmF0b3IgKG5vdCBzdXBwb3J0ZWQgaW4gdGhlIGJyb3dzZXIgeWV0KSwKLy8gd2hlcmUgdGhlcmUgYXJlIG11bHRpcGxlIGFzeW5jaHJvbm91cyBzdGVwcyBhbmQgdGhlIFB5dGhvbiBzaWRlIGlzIGdvaW5nCi8vIHRvIHBvbGwgZm9yIGNvbXBsZXRpb24gb2YgZWFjaCBzdGVwLgovLyBUaGlzIHVzZXMgYSBQcm9taXNlIHRvIGJsb2NrIHRoZSBweXRob24gc2lkZSBvbiBjb21wbGV0aW9uIG9mIGVhY2ggc3RlcCwKLy8gdGhlbiBwYXNzZXMgdGhlIHJlc3VsdCBvZiB0aGUgcHJldmlvdXMgc3RlcCBhcyB0aGUgaW5wdXQgdG8gdGhlIG5leHQgc3RlcC4KZnVuY3Rpb24gX3VwbG9hZEZpbGVzQ29udGludWUob3V0cHV0SWQpIHsKICBjb25zdCBvdXRwdXRFbGVtZW50ID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQob3V0cHV0SWQpOwogIGNvbnN0IHN0ZXBzID0gb3V0cHV0RWxlbWVudC5zdGVwczsKCiAgY29uc3QgbmV4dCA9IHN0ZXBzLm5leHQob3V0cHV0RWxlbWVudC5sYXN0UHJvbWlzZVZhbHVlKTsKICByZXR1cm4gUHJvbWlzZS5yZXNvbHZlKG5leHQudmFsdWUucHJvbWlzZSkudGhlbigodmFsdWUpID0+IHsKICAgIC8vIENhY2hlIHRoZSBsYXN0IHByb21pc2UgdmFsdWUgdG8gbWFrZSBpdCBhdmFpbGFibGUgdG8gdGhlIG5leHQKICAgIC8vIHN0ZXAgb2YgdGhlIGdlbmVyYXRvci4KICAgIG91dHB1dEVsZW1lbnQubGFzdFByb21pc2VWYWx1ZSA9IHZhbHVlOwogICAgcmV0dXJuIG5leHQudmFsdWUucmVzcG9uc2U7CiAgfSk7Cn0KCi8qKgogKiBHZW5lcmF0b3IgZnVuY3Rpb24gd2hpY2ggaXMgY2FsbGVkIGJldHdlZW4gZWFjaCBhc3luYyBzdGVwIG9mIHRoZSB1cGxvYWQKICogcHJvY2Vzcy4KICogQHBhcmFtIHtzdHJpbmd9IGlucHV0SWQgRWxlbWVudCBJRCBvZiB0aGUgaW5wdXQgZmlsZSBwaWNrZXIgZWxlbWVudC4KICogQHBhcmFtIHtzdHJpbmd9IG91dHB1dElkIEVsZW1lbnQgSUQgb2YgdGhlIG91dHB1dCBkaXNwbGF5LgogKiBAcmV0dXJuIHshSXRlcmFibGU8IU9iamVjdD59IEl0ZXJhYmxlIG9mIG5leHQgc3RlcHMuCiAqLwpmdW5jdGlvbiogdXBsb2FkRmlsZXNTdGVwKGlucHV0SWQsIG91dHB1dElkKSB7CiAgY29uc3QgaW5wdXRFbGVtZW50ID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaW5wdXRJZCk7CiAgaW5wdXRFbGVtZW50LmRpc2FibGVkID0gZmFsc2U7CgogIGNvbnN0IG91dHB1dEVsZW1lbnQgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChvdXRwdXRJZCk7CiAgb3V0cHV0RWxlbWVudC5pbm5lckhUTUwgPSAnJzsKCiAgY29uc3QgcGlja2VkUHJvbWlzZSA9IG5ldyBQcm9taXNlKChyZXNvbHZlKSA9PiB7CiAgICBpbnB1dEVsZW1lbnQuYWRkRXZlbnRMaXN0ZW5lcignY2hhbmdlJywgKGUpID0+IHsKICAgICAgcmVzb2x2ZShlLnRhcmdldC5maWxlcyk7CiAgICB9KTsKICB9KTsKCiAgY29uc3QgY2FuY2VsID0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudCgnYnV0dG9uJyk7CiAgaW5wdXRFbGVtZW50LnBhcmVudEVsZW1lbnQuYXBwZW5kQ2hpbGQoY2FuY2VsKTsKICBjYW5jZWwudGV4dENvbnRlbnQgPSAnQ2FuY2VsIHVwbG9hZCc7CiAgY29uc3QgY2FuY2VsUHJvbWlzZSA9IG5ldyBQcm9taXNlKChyZXNvbHZlKSA9PiB7CiAgICBjYW5jZWwub25jbGljayA9ICgpID0+IHsKICAgICAgcmVzb2x2ZShudWxsKTsKICAgIH07CiAgfSk7CgogIC8vIENhbmNlbCB1cGxvYWQgaWYgdXNlciBoYXNuJ3QgcGlja2VkIGFueXRoaW5nIGluIHRpbWVvdXQuCiAgY29uc3QgdGltZW91dFByb21pc2UgPSBuZXcgUHJvbWlzZSgocmVzb2x2ZSkgPT4gewogICAgc2V0VGltZW91dCgoKSA9PiB7CiAgICAgIHJlc29sdmUobnVsbCk7CiAgICB9LCBGSUxFX0NIQU5HRV9USU1FT1VUX01TKTsKICB9KTsKCiAgLy8gV2FpdCBmb3IgdGhlIHVzZXIgdG8gcGljayB0aGUgZmlsZXMuCiAgY29uc3QgZmlsZXMgPSB5aWVsZCB7CiAgICBwcm9taXNlOiBQcm9taXNlLnJhY2UoW3BpY2tlZFByb21pc2UsIHRpbWVvdXRQcm9taXNlLCBjYW5jZWxQcm9taXNlXSksCiAgICByZXNwb25zZTogewogICAgICBhY3Rpb246ICdzdGFydGluZycsCiAgICB9CiAgfTsKCiAgaWYgKCFmaWxlcykgewogICAgcmV0dXJuIHsKICAgICAgcmVzcG9uc2U6IHsKICAgICAgICBhY3Rpb246ICdjb21wbGV0ZScsCiAgICAgIH0KICAgIH07CiAgfQoKICBjYW5jZWwucmVtb3ZlKCk7CgogIC8vIERpc2FibGUgdGhlIGlucHV0IGVsZW1lbnQgc2luY2UgZnVydGhlciBwaWNrcyBhcmUgbm90IGFsbG93ZWQuCiAgaW5wdXRFbGVtZW50LmRpc2FibGVkID0gdHJ1ZTsKCiAgZm9yIChjb25zdCBmaWxlIG9mIGZpbGVzKSB7CiAgICBjb25zdCBsaSA9IGRvY3VtZW50LmNyZWF0ZUVsZW1lbnQoJ2xpJyk7CiAgICBsaS5hcHBlbmQoc3BhbihmaWxlLm5hbWUsIHtmb250V2VpZ2h0OiAnYm9sZCd9KSk7CiAgICBsaS5hcHBlbmQoc3BhbigKICAgICAgICBgKCR7ZmlsZS50eXBlIHx8ICduL2EnfSkgLSAke2ZpbGUuc2l6ZX0gYnl0ZXMsIGAgKwogICAgICAgIGBsYXN0IG1vZGlmaWVkOiAkewogICAgICAgICAgICBmaWxlLmxhc3RNb2RpZmllZERhdGUgPyBmaWxlLmxhc3RNb2RpZmllZERhdGUudG9Mb2NhbGVEYXRlU3RyaW5nKCkgOgogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAnbi9hJ30gLSBgKSk7CiAgICBjb25zdCBwZXJjZW50ID0gc3BhbignMCUgZG9uZScpOwogICAgbGkuYXBwZW5kQ2hpbGQocGVyY2VudCk7CgogICAgb3V0cHV0RWxlbWVudC5hcHBlbmRDaGlsZChsaSk7CgogICAgY29uc3QgZmlsZURhdGFQcm9taXNlID0gbmV3IFByb21pc2UoKHJlc29sdmUpID0+IHsKICAgICAgY29uc3QgcmVhZGVyID0gbmV3IEZpbGVSZWFkZXIoKTsKICAgICAgcmVhZGVyLm9ubG9hZCA9IChlKSA9PiB7CiAgICAgICAgcmVzb2x2ZShlLnRhcmdldC5yZXN1bHQpOwogICAgICB9OwogICAgICByZWFkZXIucmVhZEFzQXJyYXlCdWZmZXIoZmlsZSk7CiAgICB9KTsKICAgIC8vIFdhaXQgZm9yIHRoZSBkYXRhIHRvIGJlIHJlYWR5LgogICAgbGV0IGZpbGVEYXRhID0geWllbGQgewogICAgICBwcm9taXNlOiBmaWxlRGF0YVByb21pc2UsCiAgICAgIHJlc3BvbnNlOiB7CiAgICAgICAgYWN0aW9uOiAnY29udGludWUnLAogICAgICB9CiAgICB9OwoKICAgIC8vIFVzZSBhIGNodW5rZWQgc2VuZGluZyB0byBhdm9pZCBtZXNzYWdlIHNpemUgbGltaXRzLiBTZWUgYi82MjExNTY2MC4KICAgIGxldCBwb3NpdGlvbiA9IDA7CiAgICB3aGlsZSAocG9zaXRpb24gPCBmaWxlRGF0YS5ieXRlTGVuZ3RoKSB7CiAgICAgIGNvbnN0IGxlbmd0aCA9IE1hdGgubWluKGZpbGVEYXRhLmJ5dGVMZW5ndGggLSBwb3NpdGlvbiwgTUFYX1BBWUxPQURfU0laRSk7CiAgICAgIGNvbnN0IGNodW5rID0gbmV3IFVpbnQ4QXJyYXkoZmlsZURhdGEsIHBvc2l0aW9uLCBsZW5ndGgpOwogICAgICBwb3NpdGlvbiArPSBsZW5ndGg7CgogICAgICBjb25zdCBiYXNlNjQgPSBidG9hKFN0cmluZy5mcm9tQ2hhckNvZGUuYXBwbHkobnVsbCwgY2h1bmspKTsKICAgICAgeWllbGQgewogICAgICAgIHJlc3BvbnNlOiB7CiAgICAgICAgICBhY3Rpb246ICdhcHBlbmQnLAogICAgICAgICAgZmlsZTogZmlsZS5uYW1lLAogICAgICAgICAgZGF0YTogYmFzZTY0LAogICAgICAgIH0sCiAgICAgIH07CiAgICAgIHBlcmNlbnQudGV4dENvbnRlbnQgPQogICAgICAgICAgYCR7TWF0aC5yb3VuZCgocG9zaXRpb24gLyBmaWxlRGF0YS5ieXRlTGVuZ3RoKSAqIDEwMCl9JSBkb25lYDsKICAgIH0KICB9CgogIC8vIEFsbCBkb25lLgogIHlpZWxkIHsKICAgIHJlc3BvbnNlOiB7CiAgICAgIGFjdGlvbjogJ2NvbXBsZXRlJywKICAgIH0KICB9Owp9CgpzY29wZS5nb29nbGUgPSBzY29wZS5nb29nbGUgfHwge307CnNjb3BlLmdvb2dsZS5jb2xhYiA9IHNjb3BlLmdvb2dsZS5jb2xhYiB8fCB7fTsKc2NvcGUuZ29vZ2xlLmNvbGFiLl9maWxlcyA9IHsKICBfdXBsb2FkRmlsZXMsCiAgX3VwbG9hZEZpbGVzQ29udGludWUsCn07Cn0pKHNlbGYpOwo=",
              "ok": true,
              "headers": [
                [
                  "content-type",
                  "application/javascript"
                ]
              ],
              "status": 200,
              "status_text": ""
            }
          },
          "base_uri": "https://localhost:8080/",
          "height": 74
        },
        "outputId": "5b006f70-98b7-4d9f-f76d-511341efb6e0"
      },
      "source": [
        "from google.colab import files\n",
        "uploaded = files.upload()"
      ],
      "execution_count": 3,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/html": [
              "\n",
              "     <input type=\"file\" id=\"files-c13959c7-0c0a-4ffa-844c-edfcc0ce9ff0\" name=\"files[]\" multiple disabled />\n",
              "     <output id=\"result-c13959c7-0c0a-4ffa-844c-edfcc0ce9ff0\">\n",
              "      Upload widget is only available when the cell has been executed in the\n",
              "      current browser session. Please rerun this cell to enable.\n",
              "      </output>\n",
              "      <script src=\"/nbextensions/google.colab/files.js\"></script> "
            ],
            "text/plain": [
              "<IPython.core.display.HTML object>"
            ]
          },
          "metadata": {
            "tags": []
          }
        },
        {
          "output_type": "stream",
          "text": [
            "Saving dataset.csv to dataset.csv\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "uYFjAxa9KdIS",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "import io\n",
        "df = pd.read_csv(io.BytesIO(uploaded['dataset.csv']))"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "k7fMyaURKtL4",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 336
        },
        "outputId": "e61e7fd4-13a2-4a10-8b5c-b69435027d1c"
      },
      "source": [
        "df.info()\n"
      ],
      "execution_count": 42,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 303 entries, 0 to 302\n",
            "Data columns (total 14 columns):\n",
            "age         303 non-null int64\n",
            "sex         303 non-null int64\n",
            "cp          303 non-null int64\n",
            "trestbps    303 non-null int64\n",
            "chol        303 non-null int64\n",
            "fbs         303 non-null int64\n",
            "restecg     303 non-null int64\n",
            "thalach     303 non-null int64\n",
            "exang       303 non-null int64\n",
            "oldpeak     303 non-null float64\n",
            "slope       303 non-null int64\n",
            "ca          303 non-null int64\n",
            "thal        303 non-null int64\n",
            "target      303 non-null int64\n",
            "dtypes: float64(1), int64(13)\n",
            "memory usage: 33.2 KB\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "q19YeK_jKwRL",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        ""
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "lppLrNrOK0TE",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        ""
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "4j-9MM99LFmx",
        "colab_type": "text"
      },
      "source": [
        "\n",
        "# **Data** **Processing** "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "FXy7Sd_PLYtE",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "dataset = pd.get_dummies(df, columns = ['sex', 'cp', 'fbs', 'restecg', 'exang', 'slope', 'ca', 'thal'])\n"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "C6BOmyOELbkM",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "from sklearn.model_selection import train_test_split\n",
        "from sklearn.preprocessing import StandardScaler\n",
        "standardScaler = StandardScaler()\n",
        "columns_to_scale = ['age', 'trestbps', 'chol', 'thalach', 'oldpeak']\n",
        "dataset[columns_to_scale] = standardScaler.fit_transform(dataset[columns_to_scale])"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "VcKzJFLDLwBK",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 215
        },
        "outputId": "b3bf9c93-1838-4401-ba19-cda5d5894d89"
      },
      "source": [
        "dataset.head()\n"
      ],
      "execution_count": 37,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>age</th>\n",
              "      <th>trestbps</th>\n",
              "      <th>chol</th>\n",
              "      <th>thalach</th>\n",
              "      <th>oldpeak</th>\n",
              "      <th>target</th>\n",
              "      <th>sex_0</th>\n",
              "      <th>sex_1</th>\n",
              "      <th>cp_0</th>\n",
              "      <th>cp_1</th>\n",
              "      <th>cp_2</th>\n",
              "      <th>cp_3</th>\n",
              "      <th>fbs_0</th>\n",
              "      <th>fbs_1</th>\n",
              "      <th>restecg_0</th>\n",
              "      <th>restecg_1</th>\n",
              "      <th>restecg_2</th>\n",
              "      <th>exang_0</th>\n",
              "      <th>exang_1</th>\n",
              "      <th>slope_0</th>\n",
              "      <th>slope_1</th>\n",
              "      <th>slope_2</th>\n",
              "      <th>ca_0</th>\n",
              "      <th>ca_1</th>\n",
              "      <th>ca_2</th>\n",
              "      <th>ca_3</th>\n",
              "      <th>ca_4</th>\n",
              "      <th>thal_0</th>\n",
              "      <th>thal_1</th>\n",
              "      <th>thal_2</th>\n",
              "      <th>thal_3</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>0.952197</td>\n",
              "      <td>0.763956</td>\n",
              "      <td>-0.256334</td>\n",
              "      <td>0.015443</td>\n",
              "      <td>1.087338</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>-1.915313</td>\n",
              "      <td>-0.092738</td>\n",
              "      <td>0.072199</td>\n",
              "      <td>1.633471</td>\n",
              "      <td>2.122573</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>-1.474158</td>\n",
              "      <td>-0.092738</td>\n",
              "      <td>-0.816773</td>\n",
              "      <td>0.977514</td>\n",
              "      <td>0.310912</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>0.180175</td>\n",
              "      <td>-0.663867</td>\n",
              "      <td>-0.198357</td>\n",
              "      <td>1.239897</td>\n",
              "      <td>-0.206705</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>0.290464</td>\n",
              "      <td>-0.663867</td>\n",
              "      <td>2.082050</td>\n",
              "      <td>0.583939</td>\n",
              "      <td>-0.379244</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>0</td>\n",
              "      <td>1</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "        age  trestbps      chol   thalach  ...  thal_0  thal_1  thal_2  thal_3\n",
              "0  0.952197  0.763956 -0.256334  0.015443  ...       0       1       0       0\n",
              "1 -1.915313 -0.092738  0.072199  1.633471  ...       0       0       1       0\n",
              "2 -1.474158 -0.092738 -0.816773  0.977514  ...       0       0       1       0\n",
              "3  0.180175 -0.663867 -0.198357  1.239897  ...       0       0       1       0\n",
              "4  0.290464 -0.663867  2.082050  0.583939  ...       0       0       1       0\n",
              "\n",
              "[5 rows x 31 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 37
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "V_-wXKrxL2DV",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "y = dataset['target']\n",
        "X = dataset.drop(['target'], axis = 1)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "eF3HUkAAL24i",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "\n",
        "from sklearn.model_selection import cross_val_score\n",
        "knn_scores = []\n",
        "for k in range(1,21):\n",
        "    knn_classifier = KNeighborsClassifier(n_neighbors = k)\n",
        "    score=cross_val_score(knn_classifier,X,y,cv=10)\n",
        "    knn_scores.append(score.mean())"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ZLtaH-D3L57j",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        ""
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "uha_aLKqNI5O",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 312
        },
        "outputId": "4d75f9c1-c4f4-4ac1-c7ce-2315bc7fa84f"
      },
      "source": [
        "plt.plot([k for k in range(1, 21)], knn_scores, color = 'red')\n",
        "for i in range(1,21):\n",
        "    plt.text(i, knn_scores[i-1], (i, knn_scores[i-1]))\n",
        "plt.xticks([i for i in range(1, 21)])\n",
        "plt.xlabel('Number of Neighbors (K)')\n",
        "plt.ylabel('Scores')\n",
        "plt.title('K Neighbors Classifier scores for different K values')"
      ],
      "execution_count": 43,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "Text(0.5, 1.0, 'K Neighbors Classifier scores for different K values')"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 43
        },
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n4uPjERAQgPj4eBQWFgIAPvzwQyXvsLAwmJqaoqCgAAAwa9YsaLVahIWFITk5GaWlpXXWva7ympqa\nKvkkJiYq7v369cPZs2dr7C8VFQX+F5yF/DCupk2bUkXlSWbevHmcPXu2cr99+3auX7+e3bp1Mwq3\nceNG6vV66vV69uvXj5988kmd6ebn59PHx4f5+fksKCigj48PCwoKqoWLj4/npk2blDxiY2NJkj/8\n8EO1MpBkRUUFfX19ef78eZaVlTEiIoInTpwgSS5cuJADBw7kvXv3SJJXr14lSRYWFjIkJIRZWVlG\n7nq9nkVFRSTJu3fvsnnz5ty3b1+1PHv27MklS5YoZezcuTP1ej337dvH5s2bP1B916xZw+TkZGq1\nWsVtwoQJTElJIUmmpKRw4sSJ1fJev34927dvT5LMycmhRqNhSUkJSbJPnz5ctGhRnXWvrbwkaWNj\nUy0/kty1axf/3//7fzX6VQIgnf8F73D1enyXOiOgovI3YdmyZXjmmWeU+44dO8LOrvq5Sl27doUQ\nAkIING/eHDk5OXWmu2XLFsTHx8PJyQmOjo6Ij4/H999/Xy2cEAK3bt0CANy8eRNubm7Vwhhy8OBB\n+Pv7w9fXFxYWFujXrx9SU6V9tBYsWIA333wTJibSK8rFxQUAsHz5cvTs2RNeXl5G7kII2NpKpw+X\nl5ejvLwcospJ4rdu3cLOnTuVGYHU1FQMGjQIQgi0bNkSN27cwJUrV+qs7+3btzFz5ky88cYbRmmn\npqZi8GBpR/DBgwfj22+/rVbfr7/+GsnJycp9RUUF7ty5g4qKCpSUlCjtVVvdaytvXbRr1w7bt29H\nRUVFneFUnmxUQUBF5W/A3bt3ceHCBfwRE9ry8nJ89dVX6Ny5c53hLl++DE/P30/G9vDwwOXLl6uF\nmz17NiZMmABPT0+MHz8eKSm/n9e1b98+REZGokuXLjhx4sR90z1//jxWrlyJ6OhodOnSRZnePnPm\nDAoLCxEXF4emTZviyy+/VOLfu3cPOp0OLi4uiI+PR4sWLYzK9+2336Jjx46oX79+nfnXVa4pU6Zg\n3LhxqFevnlHaV69ehaurKwCgcePGuHr1qpF/SUkJvv/+e/Tq1QsA4O7ujvHjx8PLywuurq6wt7dH\np06d6qx7XeUqLS1FdHQ0WrZsaSSEmJiYwN/fH8eOHavWXyoqlaiCgIrK34C8vDw4ODj8oTijRo1C\n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\n6eTJkzUyMlKjo6M1MTFRVVW//vprjY6O1m7duml0dLT+85//9Kz/mWee0datW2tISIi2bNlSJ0yY\n4Cnrz3/+s7Zv316jo6N1xYoVnvcbGRmpkZGR2qdPH8/2yc3N1bPPPlujo6M1KipKR48e7bmLYMOG\nDXr22Wdrt27dNDY2VhcuXOhZ/yOPPKLt27fXiIgIz90PJW2vmTNnaqdOnbRTp046ZcoUzcvLU1XV\nN998s8D2mjdv3kmf1ZIlSwrcNfDzzz9rr169TlrOmIoOu2ug2k+iWnyr9MqkZ8+eunLlyt93pXl5\nTpfCqrB+PavXrmXGjBnMmTOnxLCLLrrIc5+7qRpmzJhBgwYNmDBhQrCrYoxfRGSVqvYMdj1M8IQG\nuwKV2vz5zoiDb78NNWoQFxfHgAEDyM3NLdDorTBLAqqeRo0aMXbs2GBXwxhj/GZnBMpKFeLjISMD\nNm2CEnb8xhhTUdkZAWNnBMrq449hzRqYPduSAGOMMZWW3TVQFqrw8MPQti2MGRPs2hhjjDFlZmcE\nymLRIli+HF56CWrWDHZtjDHGmDIL6BkBEblYRLaIyI8iMrWI12eIyFp3ShWRI16vnSUin4vIJhHZ\nKCJtA1lXn+WfDQgLA3dcAGOMMaayCtgZAREJAV4ABgO7gRUikqSqG/OXUdXJXsvfBvTwKuJN4FFV\nXSQipwOl9yP7e/jyS/jqK3juOXAH9zHGGGMqq0CeEegN/Kiq21U1G5gLXF7C8qOARAAR6QqEquoi\nAFXNUNXfAlhX3z38MLRoAXa/uDHGmCogkIlAa2CX1/Pd7ryTiEg40A5Y7M6KAI6IyIciskZEprln\nGArH3SQiK0VkZWlD/JaLb76BxYvhr38F61PeGGNMFVBR7hoYCbyvqrnu81DgPOAvQC+gPXB94SBV\nfVlVe6pqz6ZNmwa+lg8/DE2awM03B35dxhhjzO8gkIlAOtDG63mYO68oI3EvC7h2A2vdywo5wHwg\nLiC19NWKFfDZZ3DXXVCvXlCrYowxxpSXQCYCK4BOItJORGrh7OyTCi8kIl2AxsC3hWIbiUj+Yf5A\nYGPh2N/VI49A48Zw661BrYYxxhhTngKWCLhH8hOBhcAm4F1V3SAiD4lIgteiI4G56tXXsXuJ4C9A\nsoisBwR4JVB1LdW6dZCUBJMmQf36QauGMcYYU95srAFfXHMNfP45/PQTNGoUmHUYY0wQ2FgDpqI0\n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\niYAxxhhTjf1/6SoI+q7Xn7AAAAAASUVORK5CYII=\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "tags": []
          }
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "MpRrBkuVcr60",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "knn_classifier = KNeighborsClassifier(n_neighbors = 12)\n",
        "score=cross_val_score(knn_classifier,X,y,cv=10)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "PMnu4pLML8u-",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        },
        "outputId": "20b26b3d-9b73-49a3-d1bf-bb8ee828f5c9"
      },
      "source": [
        "score.mean()\n"
      ],
      "execution_count": 45,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0.8506637004078605"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 45
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "QRIaNH4HNRkl",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        ""
      ],
      "execution_count": 0,
      "outputs": []
    }
  ]
}
