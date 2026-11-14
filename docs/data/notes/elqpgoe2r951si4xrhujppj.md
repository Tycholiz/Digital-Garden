
```ts
export class MarketstackDS {
  private baseUrl = `http://api.marketstack.com/v1/eod?access_key=${MARKETSTACK_API_KEY}`;

  private handleAxiosError(error: AxiosError) {
    if (error.code === "ERR_BAD_REQUEST") {
      throw new Error(
        `Received Axios error: ${error.response?.status} ${error.response?.statusText}`
      );
    }
  }

  public async getStockData(tickerSymbol: string) {
    try {
      const response = await axios.get(
        `${this.baseUrl}&symbols=${tickerSymbol}`
      );
      return response.data;
    } catch (error: unknown) {
      if (error instanceof AxiosError) this.handleAxiosError(error);
      else console.log(error);
    }
  }
}
```