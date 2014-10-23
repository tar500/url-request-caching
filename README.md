url-request-caching
===================

```
// википедия
    NSURL * url = [NSURL URLWithString:@"http://wikipedia.org"];
    
    // создаем запрос
    NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:url
                                                           cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                       timeoutInterval:30.0];
    
    // достаем закешированный ответ для этого запроса
    NSCachedURLResponse * cachedResponse = [[NSURLCache sharedURLCache] cachedResponseForRequest:request];
    NSHTTPURLResponse *response = (NSHTTPURLResponse *)cachedResponse.response;
    
    if (response) {
        
        // устанавливаем соответствующие headers в запрос, если они есть в ответе
        if (response.allHeaderFields[@"last-modified"]) {
            
            NSString * lastModifiedValue = response.allHeaderFields[@"last-modified"];
            [request setValue:lastModifiedValue forHTTPHeaderField:@"If-Modified-Since"];
        }
        
        if (response.allHeaderFields[@"Etag"]) {
            
            NSString * etagValue = response.allHeaderFields[@"Etag"];
            [request setValue:etagValue forHTTPHeaderField:@"If-None-Match"];
        }
    }
```
