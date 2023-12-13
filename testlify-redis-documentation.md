---


---

<h1 id="redis-caching-for-testlify">Redis caching for Testlify</h1>
<p>Currently, we’re using a <code>cache-aside</code> pattern i.e. we first check if the data exists in the cache, if not then we fetch it from the database and store it in the cache for future use.</p>
<pre class=" language-mermaid"><svg id="mermaid-svg-0sXvpBphSMaQ78st" width="100%" xmlns="http://www.w3.org/2000/svg" height="501" style="max-width: 726px;" viewBox="-50 -10 726 501"><style>#mermaid-svg-0sXvpBphSMaQ78st{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#000000;}#mermaid-svg-0sXvpBphSMaQ78st .error-icon{fill:#552222;}#mermaid-svg-0sXvpBphSMaQ78st .error-text{fill:#552222;stroke:#552222;}#mermaid-svg-0sXvpBphSMaQ78st .edge-thickness-normal{stroke-width:2px;}#mermaid-svg-0sXvpBphSMaQ78st .edge-thickness-thick{stroke-width:3.5px;}#mermaid-svg-0sXvpBphSMaQ78st .edge-pattern-solid{stroke-dasharray:0;}#mermaid-svg-0sXvpBphSMaQ78st .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-svg-0sXvpBphSMaQ78st .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-svg-0sXvpBphSMaQ78st .marker{fill:#666;stroke:#666;}#mermaid-svg-0sXvpBphSMaQ78st .marker.cross{stroke:#666;}#mermaid-svg-0sXvpBphSMaQ78st svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-svg-0sXvpBphSMaQ78st .actor{stroke:hsl(0,0%,83%);fill:#eee;}#mermaid-svg-0sXvpBphSMaQ78st text.actor > tspan{fill:#333;stroke:none;}#mermaid-svg-0sXvpBphSMaQ78st .actor-line{stroke:#666;}#mermaid-svg-0sXvpBphSMaQ78st .messageLine0{stroke-width:1.5;stroke-dasharray:none;stroke:#333;}#mermaid-svg-0sXvpBphSMaQ78st .messageLine1{stroke-width:1.5;stroke-dasharray:2,2;stroke:#333;}#mermaid-svg-0sXvpBphSMaQ78st #arrowhead path{fill:#333;stroke:#333;}#mermaid-svg-0sXvpBphSMaQ78st .sequenceNumber{fill:white;}#mermaid-svg-0sXvpBphSMaQ78st #sequencenumber{fill:#333;}#mermaid-svg-0sXvpBphSMaQ78st #crosshead path{fill:#333;stroke:#333;}#mermaid-svg-0sXvpBphSMaQ78st .messageText{fill:#333;stroke:#333;}#mermaid-svg-0sXvpBphSMaQ78st .labelBox{stroke:hsl(0,0%,83%);fill:#eee;}#mermaid-svg-0sXvpBphSMaQ78st .labelText,#mermaid-svg-0sXvpBphSMaQ78st .labelText > tspan{fill:#333;stroke:none;}#mermaid-svg-0sXvpBphSMaQ78st .loopText,#mermaid-svg-0sXvpBphSMaQ78st .loopText > tspan{fill:#333;stroke:none;}#mermaid-svg-0sXvpBphSMaQ78st .loopLine{stroke-width:2px;stroke-dasharray:2,2;stroke:hsl(0,0%,83%);fill:hsl(0,0%,83%);}#mermaid-svg-0sXvpBphSMaQ78st .note{stroke:hsl(60,100%,23.3333333333%);fill:#ffa;}#mermaid-svg-0sXvpBphSMaQ78st .noteText,#mermaid-svg-0sXvpBphSMaQ78st .noteText > tspan{fill:#333;stroke:none;}#mermaid-svg-0sXvpBphSMaQ78st .activation0{fill:#f4f4f4;stroke:#666;}#mermaid-svg-0sXvpBphSMaQ78st .activation1{fill:#f4f4f4;stroke:#666;}#mermaid-svg-0sXvpBphSMaQ78st .activation2{fill:#f4f4f4;stroke:#666;}#mermaid-svg-0sXvpBphSMaQ78st:root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}#mermaid-svg-0sXvpBphSMaQ78st sequence{fill:apa;}</style><g></g><g><line id="actor30" x1="75" y1="5" x2="75" y2="490" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="0" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="75" dy="0">Application</tspan></text></g><g><line id="actor31" x1="351" y1="5" x2="351" y2="490" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="276" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="351" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="351" dy="0">Cache</tspan></text></g><g><line id="actor32" x1="551" y1="5" x2="551" y2="490" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="476" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="551" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="551" dy="0">Database</tspan></text></g><defs><marker id="arrowhead" refX="9" refY="5" markerUnits="userSpaceOnUse" markerWidth="12" markerHeight="12" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z"></path></marker></defs><defs><marker id="crosshead" markerWidth="15" markerHeight="8" orient="auto" refX="16" refY="4"><path fill="black" stroke="#000000" stroke-width="1px" d="M 9,2 V 6 L16,4 Z" style="stroke-dasharray: 0, 0;"></path><path fill="none" stroke="#000000" stroke-width="1px" d="M 0,1 L 6,7 M 6,1 L 0,7" style="stroke-dasharray: 0, 0;"></path></marker></defs><defs><marker id="filled-head" refX="18" refY="7" markerWidth="20" markerHeight="28" orient="auto"><path d="M 18,7 L9,13 L14,7 L9,1 Z"></path></marker></defs><defs><marker id="sequencenumber" refX="15" refY="15" markerWidth="60" markerHeight="40" orient="auto"><circle cx="15" cy="15" r="6"></circle></marker></defs><text x="213" y="80" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Check if data is in cache</text><line x1="75" y1="113" x2="351" y2="113" class="messageLine0" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="fill: none;"></line><text x="213" y="173" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Return cached data</text><line x1="351" y1="206" x2="75" y2="206" class="messageLine1" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="313" y="266" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Request data</text><line x1="75" y1="299" x2="551" y2="299" class="messageLine0" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="fill: none;"></line><text x="313" y="314" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Return data</text><line x1="551" y1="347" x2="75" y2="347" class="messageLine1" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="stroke-dasharray: 3, 3; fill: none;"></line><text x="213" y="362" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="messageText" dy="1em" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">Update cache with new data</text><line x1="75" y1="395" x2="351" y2="395" class="messageLine0" stroke-width="2" stroke="none" marker-end="url(#arrowhead)" style="fill: none;"></line><g><line x1="65" y1="123" x2="561" y2="123" class="loopLine"></line><line x1="561" y1="123" x2="561" y2="405" class="loopLine"></line><line x1="65" y1="405" x2="561" y2="405" class="loopLine"></line><line x1="65" y1="123" x2="65" y2="405" class="loopLine"></line><line x1="65" y1="221" x2="561" y2="221" class="loopLine" style="stroke-dasharray: 3, 3;"></line><polygon points="65,123 115,123 115,136 106.6,143 65,143" class="labelBox"></polygon><text x="90" y="136" text-anchor="middle" dominant-baseline="middle" alignment-baseline="middle" class="labelText" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">alt</text><text x="338" y="141" text-anchor="middle" class="loopText" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;"><tspan x="338">[Data in cache]</tspan></text><text x="313" y="239" text-anchor="middle" class="loopText" style="font-family: &quot;trebuchet ms&quot;, verdana, arial, sans-serif; font-size: 16px; font-weight: 400;">[Data not in cache]</text></g><g><rect x="0" y="425" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="457.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="75" dy="0">Application</tspan></text></g><g><rect x="276" y="425" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="351" y="457.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="351" dy="0">Cache</tspan></text></g><g><rect x="476" y="425" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="551" y="457.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle; font-size: 14px; font-weight: 400; font-family: Open-Sans, &quot;sans-serif&quot;;"><tspan x="551" dy="0">Database</tspan></text></g></svg></pre>
<blockquote>
<p><strong>NOTE</strong>: Caching can easily be disabled for debugging purposes by simply flipping the parameter <code>USE_CACHE</code> to <code>false</code></p>
</blockquote>
<h2 id="caching-for-the-public-routes">Caching for the public routes</h2>
<ul>
<li>Done using a simple caching wrapper with the default <code>TTL</code> of 5 minutes and can be changed.</li>
</ul>
<p>CODE/LOGIC:</p>
<pre class=" language-ts"><code class="prism  language-ts">
<span class="token comment">// Log-messages/Error-handling removed for brevity</span>

<span class="token keyword">async</span>  <span class="token keyword">get</span><span class="token operator">&lt;</span>T<span class="token operator">&gt;</span><span class="token punctuation">(</span>key<span class="token punctuation">:</span> <span class="token keyword">string</span><span class="token punctuation">,</span> fetcher<span class="token punctuation">:</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span>  Promise<span class="token operator">&lt;</span>T<span class="token operator">&gt;</span><span class="token punctuation">,</span> ttl<span class="token operator">=</span><span class="token keyword">this</span><span class="token punctuation">.</span>ttl<span class="token punctuation">)</span><span class="token punctuation">:</span> Promise<span class="token operator">&lt;</span>T<span class="token operator">&gt;</span> <span class="token punctuation">{</span>

<span class="token keyword">const</span> cachedValue <span class="token operator">=</span> <span class="token keyword">await</span>  <span class="token keyword">this</span><span class="token punctuation">.</span>redisClient<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span>key<span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">if</span> <span class="token punctuation">(</span>cachedValue<span class="token punctuation">)</span> <span class="token punctuation">{</span>

<span class="token keyword">return</span>  JSON<span class="token punctuation">.</span><span class="token function">parse</span><span class="token punctuation">(</span>cachedValue<span class="token punctuation">)</span> <span class="token keyword">as</span>  T<span class="token punctuation">;</span>

<span class="token punctuation">}</span>

  

<span class="token keyword">const</span> result <span class="token operator">=</span> <span class="token keyword">await</span>  <span class="token function">fetcher</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">await</span> <span class="token keyword">this</span><span class="token punctuation">.</span>redisClient<span class="token punctuation">.</span><span class="token keyword">set</span><span class="token punctuation">(</span>key<span class="token punctuation">,</span> JSON<span class="token punctuation">.</span><span class="token function">stringify</span><span class="token punctuation">(</span>result<span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token string">'EX'</span><span class="token punctuation">,</span> ttl<span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token keyword">return</span> result<span class="token punctuation">;</span>

<span class="token punctuation">}</span>

</code></pre>
<p>USAGE:</p>
<pre class=" language-ts"><code class="prism  language-ts">
<span class="token comment">// previous code</span>

<span class="token keyword">const</span>  data <span class="token operator">=</span> <span class="token keyword">await</span>  <span class="token keyword">this</span><span class="token punctuation">.</span>exampleRepository<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

  

<span class="token comment">// new code</span>

<span class="token keyword">const</span>  cacheKey <span class="token operator">=</span> <span class="token template-string"><span class="token string">`example:</span><span class="token interpolation"><span class="token interpolation-punctuation punctuation">${</span>id<span class="token interpolation-punctuation punctuation">}</span></span><span class="token string">`</span></span><span class="token punctuation">;</span>

<span class="token keyword">const</span>  data <span class="token operator">=</span> <span class="token keyword">await</span>  <span class="token keyword">this</span><span class="token punctuation">.</span>cacheService<span class="token punctuation">.</span><span class="token keyword">get</span><span class="token punctuation">(</span>cacheKey<span class="token punctuation">,</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span>  <span class="token keyword">this</span><span class="token punctuation">.</span>exampleRepository<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>id<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

  

</code></pre>
<p>Currently, we’ve implemented caching using above scheme for the following routes:</p>
<pre><code>
GET /v1/language --&gt; language list
GET /v1/test/library/filter?language= --&gt; Test library list on language basis
GET /v1/assessment/coding/language --&gt; coding language list
GET /v1/public/language/userType/{userType} -&gt;&gt; language list for EMPLOYER/CANDIDATE 
GET /v1/public/language/{code} --&gt; translation text e.g en,fr
GET /v1/question? --&gt; questions
GET /v1/test/library?limit= --&gt; Test library list



Extra:
`organization` lookup in jwt-auth middleware
`userWorkspaceProfile` lookup in jwt-auth middleware
</code></pre>
<h2 id="caching-for-the-authenticated-routes">Caching for the authenticated routes</h2>
<ul>
<li>
<p>Similar wrapper but now requires a <code>user_id</code> to be passed as a parameter.</p>
</li>
<li>
<p>The <code>user_id</code> is used as a key to store the data in redis <code>hset</code></p>
</li>
</ul>
<blockquote>
<p>What is hset?</p>
</blockquote>
<p>Basically, it is a hash set. It is a data structure that stores data in key-value pairs. It is similar to a dictionary in Python or an object in Javascript. The key is a string and the value can be any data type. The key is unique and the value can be represented as a string or a number.</p>
<p>Example:</p>
<pre class=" language-sh"><code class="prism  language-sh">
hset &lt;key&gt; &lt;field&gt; &lt;value&gt; ...

</code></pre>
<pre class=" language-sh"><code class="prism  language-sh">
hset  user:1  name  "John"

hset  user:1  age  30

hset  user:1  /v1/assessments/1  {"id":  1,  "name":  "Test 1"}

</code></pre>
<p>To get the value of a key, we use the <code>hgetall</code> command.</p>
<pre class=" language-sh"><code class="prism  language-sh">
hgetall  user:1

</code></pre>
<p>Using the above command will return all the field-value pairs of the key <code>user:1</code> in the form of an object.</p>
<pre class=" language-sh"><code class="prism  language-sh">
{

name:  "John",

age:  30,

/v1/assessments/1:  {"id":  1,  "name":  "Test 1"}

}

</code></pre>
<p>Now we can simply filter and look for the required field to delete and modify it.</p>
<h2 id="invalidating-the-cache">Invalidating the cache</h2>
<p>The Cache invalidation will be done using the <code>invalidate</code> wrapper, which takes the <code>user_id</code> and the <code>field</code> to be deleted as parameters.</p>
<pre class=" language-js"><code class="prism  language-js">
<span class="token function">invalidate</span><span class="token punctuation">(</span>user_id<span class="token punctuation">,</span> field<span class="token punctuation">,</span> <span class="token keyword">async</span> <span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">=&gt;</span>  <span class="token function">fetcher</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token comment">// fetcher() is the actual controller function</span>

</code></pre>
<ul>
<li>
<p>It first checks if the key exists in the redis store.</p>
</li>
<li>
<p>Then using <code>hgetall</code> it gets all the field-value pairs of the key.</p>
</li>
<li>
<p>Then we run a simple loop to check if the field to delete exists in the field, if so delete the field-value pair.</p>
</li>
</ul>
<p>For more complex invalidation we can leverage lua scripts to run the loop on the redis server itself, and do complex operations like deleting all the fields that start with a particular string, etc completely on the redis server itself.</p>
<p>Example:</p>
<pre class=" language-lua"><code class="prism  language-lua">
EVAL  <span class="token string">"return redis.call('del', unpack(redis.call('user:1', ARGV[1])))"</span> <span class="token number">0</span> <span class="token operator">/</span>v1<span class="token operator">/</span>orgId<span class="token punctuation">:</span>76st2<span class="token operator">*</span>

</code></pre>

