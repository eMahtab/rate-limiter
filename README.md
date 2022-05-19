# Rate Limiter

1. Leaky Bucket
2. Token Bucket
3. Sliding Window
4. Fixed Window

## Token Bucket

![Token Bucket Algorithm](token-bucket-algorithm.png?raw=true)

```java
public class TokenBucket {
	private final long bucketCapacity;
	private final long refillRate;
	
	private double currentTokensInBucket;
	private long lastRefillTimestamp;
	
	public TokenBucket(long bucketCapacity, long refillRate) {
		this.bucketCapacity = bucketCapacity;
		this.refillRate = refillRate;
		currentTokensInBucket = bucketCapacity;
		lastRefillTimestamp = System.nanoTime();
	}
	
	public synchronized boolean isRequestAllowed(int tokens) {
		refill();
		if (currentTokensInBucket >= tokens) {
			currentTokensInBucket -= tokens;
			return true;
		}
		return false;
	}
	
	private void refill() {
		long now = System.nanoTime();
		double tokensToAdd = (now - lastRefillTimestamp) * refillRate / 1e9;
		currentTokensInBucket = Math.min(currentTokensInBucket + tokensToAdd,  bucketCapacity);
		lastRefillTimestamp = now;
	}
}

```

![Token Bucket Example](token-bucket-example.png?raw=true)

## Fixed window and Sliding window algorithm
![Fixed Window Sliding Window Algorithm](fixed-window-sliding-window-algorithm.PNG)


# References :
System Design Interview - Rate Limiting  : https://www.youtube.com/watch?v=FU4WlwfS3G0


